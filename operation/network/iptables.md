# iptables
iptables 中可以灵活的做各种网络地址转换(NAT), 网络地址主要有两种: SNAT(source network address translation, 发送方网络地址), DNAT(destination network address translation, 接收方网络地址).   
`MASQUERADE` 是 SNAT 中的一种特例, 可实现自动化的 SNAT. 如: `iptables -t nat -A POSTROUTING -j MASQUERADE` 这个命令在实际解析时会将 `MASQUERADE` 自动转换为当前机器的出口 IP 做为 SNAT 的值来使用.    


## FAQs
* 将本机端口转发到外部端口:   
    ```bash  
    local_port=8001
    target=192.168.192.2:80
    sysctl -w net.ipv4.conf.all.route_localnet=1
    iptables -t nat -A OUTPUT -p tcp --dport ${local_port} -j DNAT --to-destination ${target}  
    iptables -t nat -A POSTROUTING -j MASQUERADE

    ```
    
* 删除设置
    ```bash
    ## add
    sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    ## list 
    sudo iptables -t nat -v -L -n --line-number
    ## delete
    # sudo iptables -t nat -D {chain-name} {rule-line-number}
    sudo iptables -t nat -D INPUT 1
    sudo iptables -t nat -D OUTPUT 2
    sudo iptables -t nat -D PREROUTING 3
    ```    
    
    Refs: [Linux iptables delete prerouting rule command](https://www.cyberciti.biz/faq/linux-iptables-delete-prerouting-rule-command/)

## Refs
* [IPtables中SNAT、DNAT和MASQUERADE的含义](https://blog.csdn.net/jk110333/article/details/8229828)    

