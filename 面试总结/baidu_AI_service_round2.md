### 业务
1. sip,mrcp通信的基本流程：
a. sip消息中有哪些字段是必须的,唯一标识字段是什么
b. 在哪里协商mrcp及rtp会话信息
c. udp丢包对识别结果的影响
d. ims如何处理接收到的rtp包，收到立刻发还是做缓存

2. rtp如何对抗udp丢包与乱序？
1. jitter buffer
https://www.cnblogs.com/talkaudiodev/p/8025242.html
2. 如何排查乱序和重排
https://blog.csdn.net/gllilin/article/details/73557966?spm=1001.2014.3001.5501
https://blog.csdn.net/gllilin/article/details/73571993?spm=1001.2014.3001.5501

### 网络
1. TCP的流量控制
windows size value
windows size Scaling factor 只在3次握手时得知
真实的滑动窗口大小为:windows size value * windows size Scaling factor
