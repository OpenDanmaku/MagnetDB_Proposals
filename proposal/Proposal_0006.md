MD传输层通信协议简述
===
Magnet Danmaku 传输层通信协议。版本1。

基础通信通过基于流的协议完成。建议的选项为TCP和uTP。
在流协议的基础上，实现握手和封包的格式。

握手
---
在连接建立后先进行握手。双方发送如下数据：  
`"MD"|协议版本号(=1)|节点ed25519公钥（即节点id）`  
如果协议版本号不兼容则终止连接。如果无问题则直接开始传输数据。数据传输不必等待握手完成。

封包
---
包格式如下：  
`数据长度 in bytes (2 bytes, big endian)|分离的ed25519签名(32 bytes)|数据`  
特殊情况：当数据长度==0xffff时，该段数据不立即终止，继续读下一段数据，直到一个数据长度在0和0xfffe之间的包为止。