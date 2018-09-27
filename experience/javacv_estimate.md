#### JavaCV cvEstimateRigidTransform函数使用心得
@Date 2018.09.27

##### 函数定义

* 对应OpenCV中的estimateRigidTransform函数
    * 此函数用作根据变换矩阵对图片进行指定大小的变换
    * Mat estimateRigidTransform(InputArray src,InputArray dst,bool fullAffine)
    * src : 变换前的图片关键点
    * dst : 期望变换后的图片关键点
    * fullAffine : 1(全仿射变换), 0(带有约束的仿射变换)
    * 返回值 : 得到变换后的图片MAT
    
##### 使用场景

* 项目中实际场景可能为在一张图片中, 切出人脸图片. 但是人脸图片是根据坐标切割, 图片的像素大小是不固定的. 
现在可以根据矩阵变换, 把所有人脸图片归一到指定大小, 比如80 * 80

##### JavaCV

* 在JavaCV中参数传递都是以Mat对象传递
* 需要提前开辟好关键点对象的空间

```java
// pointer存储关键点矩阵信息
Point2f pointerX = new Point2f();
Point2f pointerY = new Point2f();
// 转换pointer到mat
Mat matSrcA = new Mat(3, 2, CV_32FC1, pointerX);
Mat matSrcB = new Mat(3, 2, CV_32FC1, pointerY);

// Javacv中返回值需要提前开辟传入
Point2f cv = new Point2f();
Mat cvEstimateOut = new Mat(2, 3, CV_32FC1, cv);
// 关键点矩阵转换
cvEstimateRigidTransform(new CvMat(matSrcA), new CvMat(matSrcB), new CvMat(cvEstimateOut), 0);

// JavaCV函数 : 根据前后关键点矩阵, 进行指定图片大小的变换
Mat result = new Mat(80, 80, CV_8UC3);
warpAffine(matImage, result, cvEstimateOut, new Size(80, 80));
```
