#### JavaCV FrameGrabber问题汇总
@Date 2018.09.27

##### FrameGrabber类

* JavaCV中FrameGrabber类可以连接直播流地址, 进行解码, 获取Frame帧信息, 常用方式如下

```java
FrameGrabber grabber = new FrameGrabber("rtsp:/192.168.0.0");
while(true) {
    Frame frame = grabber.grabImage();
    // ...
}
```

##### 问题(1)

* 在如上代码中, 若直播地址网络不通, 或者连接超时, 则会出现程序Hang住的情况, 也就是同步阻塞卡死. 因为可以增加一个超时参数进行解决.

###### 解决办法(1)

```java
FrameGrabber grabber = new FrameGrabber("rtsp:/192.168.0.0");
// 增加超时参数
grabber.setOption(TimeoutOption.STIMEOUT.key(), "5000000");
while(true) {
    Frame frame = grabber.grabImage();
    // ...
}

```

##### 问题(2)

* FrameGrabber hangs when server connection is broken
* 在问题(1)的基础上, 若刚开始网络是通的, 程序可以获取到解码后的视频帧. 而在已经运行通了的时候, 网络突然断掉, 此时上面的timeout是不生效的. 
* 同时可以参考GitHub上的提问(https://github.com/bytedeco/javacv/issues/1063)

###### 解决办法(2)

* 在GitHub上面, 作者介绍了Callback_Pointer方式, 参考(https://github.com/bytedeco/javacv/blob/master/samples/FFmpegStreamingTimeout.java)
* 但是经过测试, 发现此方式还是不能解决问题(2), 在FrameGrabber外部增加callback对原类中的对象不生效. 
* 故使用一种笨方式, 继承重写FrameGrabber类, 把超时callback增加到原代码逻辑中.

```java
public class CustomFrameGrabber extends FrameGrabber {
    /**
     * 读流超时时间 : 10秒
     */
    private static final int FRAME_READ_TIMEOUT = 10000;

    // 增加callbacjk 监听
    private final AtomicLong lastFrameTime = new AtomicLong(System.currentTimeMillis());
    private final AVIOInterruptCB.Callback_Pointer cp = new avformat.AVIOInterruptCB.Callback_Pointer() {
        @Override
        public int call(Pointer pointer) {
            // 0:continue, 1:exit
            if (lastFrameTime.get() + FRAME_READ_TIMEOUT < System.currentTimeMillis()) {
                MONITOR_LOGGER.warn("frame read timeout 10s.");
                return 1;
            }
            return 0;
        }
    };
    
    // ... 省略中间代码
    
    void startUnsafe() throws Exception {
        int ret;
        img_convert_ctx = null;
        oc = new AVFormatContext(null);
        video_c = null;
        audio_c = null;
        pkt = new AVPacket();
        pkt2 = new AVPacket();
        sizeof_pkt = pkt.sizeof();
        got_frame = new int[1];
        frameGrabbed = false;
        frame = new Frame();
        timestamp = 0;
        frameNumber = 0;

        pkt2.size(0);

        // Open video file
        AVInputFormat f = null;
        if (format != null && format.length() > 0) {
            if ((f = av_find_input_format(format)) == null) {
                throw new Exception("av_find_input_format() error: Could not find input format \"" + format + "\".");
            }
        }
        AVDictionary options = new AVDictionary(null);
        if (frameRate > 0) {
            AVRational r = av_d2q(frameRate, 1001000);
            av_dict_set(options, "framerate", r.num() + "/" + r.den(), 0);
        }
        if (pixelFormat >= 0) {
            av_dict_set(options, "pixel_format", av_get_pix_fmt_name(pixelFormat).getString(), 0);
        } else if (imageMode != ImageMode.RAW) {
            av_dict_set(options, "pixel_format", imageMode == ImageMode.COLOR ? "bgr24" : "gray8", 0);
        }
        if (imageWidth > 0 && imageHeight > 0) {
            av_dict_set(options, "video_size", imageWidth + "x" + imageHeight, 0);
        }
        if (sampleRate > 0) {
            av_dict_set(options, "sample_rate", "" + sampleRate, 0);
        }
        if (audioChannels > 0) {
            av_dict_set(options, "channels", "" + audioChannels, 0);
        }
        for (Entry<String, String> e : this.options.entrySet()) {
            av_dict_set(options, e.getKey(), e.getValue(), 0);
        }
        if (inputStream != null) {
            if (readCallback == null) {
                readCallback = new CustomFFmpegFrameGrabber.ReadCallback();
            }
            if (seekCallback == null) {
                seekCallback = new CustomFFmpegFrameGrabber.SeekCallback();
            }
            if (!inputStream.markSupported()) {
                inputStream = new BufferedInputStream(inputStream, 1024 * 1024);
            }
            inputStream.mark(1024 * 1024);
            oc = avformat_alloc_context();
            avio = avio_alloc_context(new BytePointer(av_malloc(4096)), 4096, 0, oc, readCallback, null, seekCallback);
            oc.pb(avio);

            filename = inputStream.toString();
            inputStreams.put(oc, inputStream);
        }
        // 2018.09.07 add 此处一行为新增代码, 意思是每次取到帧时, 都更新一下时间. 若遇到网络断开, hang住时, 则可用此时间判断超时
        lastFrameTime.set(System.currentTimeMillis());
        oc = avformat_alloc_context();
        avformat.AVIOInterruptCB cb = new avformat.AVIOInterruptCB();
        cb.callback(cp);
        oc.interrupt_callback(cb);
        // 省略后续代码...
    }
    
    public Frame grabFrame(boolean doAudio, boolean doVideo, boolean processImage, boolean keyFrames) throws Exception {
        if (oc == null || oc.isNull()) {
            throw new Exception("Could not grab: No AVFormatContext. (Has start() been called?)");
        } else if ((!doVideo || video_st == null) && (!doAudio || audio_st == null)) {
            return null;
        }
        // 2018.09.07 add 获取帧时, 更新最新时间戳
        lastFrameTime.set(System.currentTimeMillis());
        
        // 省略后续代码...
    }
}
```

