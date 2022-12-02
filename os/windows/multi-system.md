# 多系统
## FAQs
* Windows 上编辑的文本文件在 *nix 系统上在行末会出现 `^M`. 参考 [What does \^M character mean in Vim?](https://stackoverflow.com/questions/5843495/what-does-m-character-mean-in-vim)

    Windows 的换行符为 0xD0xA(`\r\n`), *nix 系统使用 0xA(`\n`) 做为换行符. `^M` 为 `\r` 的显示. 
    可使用 `dos2unix` 命令修复. 或在 VIM 中使用 `set ffs=unix,dos` 指令.
    
* 在装了多系统的电脑上时间会出现错乱
    * 起因

        在使用多系统的机器上, *nix 系统以当前主板 CMOS 时间做为国际协调时间(UTC), 再根据系统设置的时区来最终确定当前系统时间. 而 Windows 则直接把 CMOS 时间认定为当前显示时间, 不进行时区转换, 从而导致了时间显示不同步的问题.
        
    * 解决
    
        让 Windows 系统开机时进行网络时间同步:
        
          1. 确保 Windows Time(W32Time) 服务的启动类型为非 "禁用" 状态.
          2. 设置 Internet 时间同步服务器: 系统默认的时间同步服务器不怎么稳定, 可以在 "日期和时间" - "Internet 时间" - "更改设置" 下, 修改服务器为: `0.pool.ntp.org`.
          3. 将下面的脚本添加到开机启动中:

              ```dos
                rem 开启服务
                net start W32Time
                rem 运行同步
                w32tm /resync
              ```
              
    * 参考
        * [如何修正因Windows和Linux或者Mac双系统引起的系统时间错误](https://www.wikai.info/2011_01_477.html)
        * [如何修改windows下的时间同步间隔](http://blog.csdn.net/fffygapl/article/details/8475619)
