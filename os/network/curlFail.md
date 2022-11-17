# 大部分网页无法连接，能 ping 不能 curl. 换系统不行 换网络就可以
- date: 2022-11-17
- lastmod: 2022-11-17

bing 无法访问，baidu 却可以
```bash
$ cat /etc/resolv.conf 
nameserver 2400:3200::1
nameserver 223.5.5.5
nameserver 166.111.68.130

$ dig www.bing.com +short
www-www.bing.com.trafficmanager.net.
cn-bing-com.cn.a-0001.a-msedge.net.
a-0001.a-msedge.net.
204.79.197.200
13.107.21.200

$ ping 204.79.197.200
PING 204.79.197.200 (204.79.197.200) 56(84) 字节的数据。64 字节，来自 204.79.197.200: icmp_seq=1 ttl=108 时间=121 毫秒64 字节，来自 204.79.197.200: icmp_seq=2 ttl=108 时间=157 毫秒64 字节，来自 204.79.197.200: icmp_seq=3 ttl=108 时间=77.6 毫秒64 字节，来自 204.79.197.200: icmp_seq=4 ttl=108 时间=311 毫秒64 字节，来自 204.79.197.200: icmp_seq=5 ttl=108 时间=125 毫秒64 字节，来自 204.79.197.200: icmp_seq=6 ttl=108 时间=149 毫秒^C
--- 204.79.197.200 ping 统计 ---
已发送 6 个包， 已接收 6 个包, 0% packet loss, time 5003ms
rtt min/avg/max/mdev = 77.626/156.653/311.185/73.562 ms

$ curl 204.79.197.200
curl: (28) Failed to connect to 204.79.197.200 port 80 after 129391 ms: Couldn't connect to server
```

# 相关阅读

[I can ping a website, but I cannot access the website using the IP number or the actual web address, help?  2013](https://answers.microsoft.com/en-us/ie/forum/all/i-can-ping-a-website-but-i-cannot-access-the/1f3475cc-9815-453b-9714-2657a891292e)：无后续，工程师6种方法：关闭代理、关闭防火墙、重置路由器、

[Can Ping websites but wont load. Rossco88 on Oct 17th, 2016](https://community.spiceworks.com/topic/1877883-can-ping-websites-but-wont-load):找相同的正常允许的路由器，导出设置进行恢复