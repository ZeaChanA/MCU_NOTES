[MQTT - 物联网消息传递标准](https://mqtt.org/)
[MQTT 版本 3.1.1 (oasis-open.org)](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html)
[MQTT 数据包：综合指南 (hivemq.com)](https://www.hivemq.com/blog/mqtt-packets-comprehensive-guide/)
[第一章 - MQTT介绍 · MQTT协议中文版 (gitbooks.io)](https://mcxiaoke.gitbooks.io/mqtt-cn/content/mqtt/01-Introduction.html)

![[mqtt-publish-subscribe.png]]
![[Pasted image 20240827232201.png]]

![[Pasted image 20240827232300.png]]
一对多
![[Pasted image 20240827232405.png]]


CONNECT报文
![[Pasted image 20240828055416.png]]


QOS服务质量
![[Pasted image 20240828055004.png]]

订阅主题
![[Pasted image 20240828060153.png]]

---

MQTT（（Message Queuing Telemetry Transport）  
消息队列遥测传输）是一种机器对机器，以数据为中心的轻量级协议，旨在用于在资源受限的环境（能源限制，限制处理能力，更高的通信延迟，高度不可靠的网络，记忆力限制，无人值守的网络操作）中运行，在这种受限环境中，MQTT比HTTP更受欢迎，与HTTP中的直接客户端服务器交互不同，MQTT在发布/订阅范例下运行，中间有MQTT代理
![[Pasted image 20241009071749.png]]
客户端可以将主题发布到MQTT代理或订阅主题，同一客户端可以发布主题X和Y，并订阅由另一个MQTT客户端发布的主题Z
![[Pasted image 20241009071938.png]]
MQTT还允许MQTT客户端和代理之间的持久连接以及不同的服务质量级别，这使得它非常适合各种受限环境，因为在这种情况下，它比HTTP更节能，更快
![[Pasted image 20241009072206.png]]
MQTT代理是一个MQTT服务器，它在与其连接的不同MQTT客户端之间传输数据，当客户端希望向代理发送数据时，它会“发布”该数据或主题，当MQTT客户端希望接收由另一个MQTT客户端发布的此数据时，它订阅此主题，然后MQTT代理将此主题传输给代理。这与HTTP不同，MQTT客户端不需要知道彼此的IP地址或端口号，他们所需要做的就是连接到同一个MQTT代理，单个MQTT代理可以处理大量MQTT客户端，并且每个代理的数量不同。