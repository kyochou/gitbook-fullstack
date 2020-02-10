# Tools

## 抓包
### Refs
* [关于手机App的Https抓包](https://blog.huoding.com/2019/05/31/741)


## Wireshark
### 过滤器
#### 捕获过滤器
用于减少抓取的报文体积. 使用 BPF 语法.
[BPF(Berkeley Packet  Filter)](https://biot.com/capstats/bpf.html), 在设备驱动级别提供抓包过滤接口. BPF 语法适用于很多抓包工具.
BPF 表达式由原语(primitives)和运算符组成.
原语由限定词(qualifiers)和限定条件组成, 如:
* Type: 类型, 如 `host www.baidu.com`;
    * host, port;
    * net: 设定子网. 如 `net 192.168.0.0/24` 等价于 `net 192.168.0.0 mask 255.255.255.0`;
    * portrange, 设定端口范围. 如 `portrange 6000-8000`;
* Dir: 网络出入方向, `src` 表示网络流入, `dst` 表示网络流出. 如 `dst port 80`;
* Proto: 协议, 如 `udp`;
* 其他: geteway, broadcast, multicast, less, greater 等;

运算符有:
* 与: `&&` 或者 `and`;
* 或: `||` 或者 `or`;
* 非: `!` 或者 `not`;

BPF 语法对数据的过滤可以精确到 `bit` 单位. 如:
* 捕获所有TCP中的RST报文: `tcp[13]&4 == 4`;
* 抓取HTTPGET报文: `port 80 and tcp[((tcp[12:1]&0xf0)>>2):4] = 0x47455420`;

#### 显示过滤器
对于已经抓取到的报文过滤显示.
任何在报文细节面板中解析出的字段名, 都可以作为过滤属性.