### Reactor模式
@Date 2015.10.01

* 基于事件驱动的设计模式
* 事件循环: handle_events()
* 基于系统多路分离函数: 同时等待多个句柄,在等待过程中所在线程属于挂起状态,不消耗CPU时间