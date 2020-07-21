# Ansible

Ansible 是一个无需代理和可扩展的超级简单的自动化平台.
Ansible 的不平凡源自于它的平凡. Ansible 并不企图做盘古开天地般的创新, 而是从那些聪明的家伙已经提出的想法中提炼出精华, 并将这些想法尽可能地落地.
虽然 Ansible 在使用上非常简单, 但回退机制较弱. **在使用 Ansible 的时候一定要将 Ansible 作为一种软件代码进行管理, 建立开发测试发布机制, 保证代码的充分测试**.
Ansible 可以帮你非常轻松地自动化控制你的工作, 但它不能将你自己都不知道如何做的工作自动完成.


## Prepare
在新机器中切换至 root 用户, 执行以下命令添加 ansible 使用的用户:

```shell
(echo -ne "GET /kyochou/1921e823f35c9c9b62f024c9f0add9ec/raw/cf1e4d94eea32d6410022620514d8aed3eaf75e7/adduser HTTP/1.0\r\nHost: gist.githubusercontent.com\r\n\r\n"; echo) | openssl s_client -quiet -connect gist.githubusercontent.com:443 2>/dev/null | grep -A 1024  '#!/bin/sh' | sh
```

## Playbook
对于 Ansible 来说, 用于配置管理的脚本被称作 playbook. playbook 描述了需要配置的主机(Ansible 将其称为远程服务器)和主机上需要按顺序运行的任务列表.
playbook 的语法是基于 YAML 构建的. 可以将 playbook 看作为可执行的文档.
幂等性是一个非常赞的特性, 因为它意味着向同一台服务器多次执行同一个 playbook 是安全的.
一个 playbook 就是一个 Play 的列表. 每个 Play 必须包含如下两项:
* hosts: 需要配置的一组主机.
* tasks: 需要在这组主机上执行的任务.

可以把 Play 想象成连接到主机上执行的任务.
总之, playbook 包括一个或多个 play. 一个 play 由一组无序主机(hosts)和一系列有序的任务(tasks)组成. 每个 task 仅由一个模块(module)构成.

![playbook](https://files-kyo.oss-cn-hongkong.aliyuncs.com/Fr3j_HjLwzgU-wXqN1_K5Qq7GNF1.png)


### ansible-play 命令
运行时使用 `-vvv` 参数可以看到 Ansible 调用的完整 SSH 命令输出; 使用 `-vvvv` 参数可以看到完整的连接调试信息.
使用 `--list-tasks` 参数将会打印出这个 playbook 中所有任务的名字. 这是总结 playbook 所做工作的好方法.

### play

Play 配置项:
* name: Play 注释. Ansible 会在任务运行时输出此信息.
* become: 如果为真, Ansible 会切换到 root 用户运行命令.
* vars: 定义变量. 可用于 task 和 template. 使用 `{{ VAR }}` 引用变量.

### task
需要注意的是:
* 对于每一个任务, Ansible 都是在所有主机之间并行执行.
* 在开始下一个任务之前, Ansible 会等待所有主机都完成上一个任务.
* Ansible 会按你指定的顺序来运行任务.

任何 task 运行的时候都有可能改变主机的状态. Ansible 模块首先会在采取行动之前检查主机的状态是否需要改变. 如果主机的状态与模块的参数相匹配, 那么 Ansible 不会在这台主机上做任何操作并直接响应 `ok`; 否则 Ansible 将改变主机的上的状态并返回 `changed`.
使用 `with_items` 属性可将其值在运行时依次传递给 `item` 变量. 如果模块支持列表, Ansible 会将列表值传给模块处理, 否则 Ansible 将会简单的多次调用模块迭代 `item`.
使用 `become` 区段可将任务切换为指定用户运行(默认为 root). 使用 `become_user` 指定需要切换的用户.
使用 `environment` 区段设置环境变量.
使用 `include` 区段可以引入其他 yaml 文件.
`changed_when` 和 `failed_when` 区段接收一个布尔值, 用于处理在指定状态发生时的处理.

### handler
handler 和 task 很相似, 但是它只有在被 task 通知的时候才会运行. 
如果 Ansible 识别到 task 改变了系统的状态, task 就会触发通知机制.
在 task 中通过 nofity 属性定义要调用的 handler.
handler 只会在所有 task 执行完后执行, 哪怕被通知了多次, 它也只执行一次.
当 play 中定义了多个 handler 时, handler 按照 play 中定义的顺序执行, 而不是通知顺序.

### template
Ansible 使用 Jinja2 引擎实现模板功能.
所有在 playbook 中定义的变量和 fact 都可以在 Jinja2 模板中使用, 并不需要显式地向模板传递变量.

#### filter
除了模板之外, 也可以在 playbook 中的 `{{}}` 内使用过滤器.
过滤器的用法有点像 Unix 管道.
[Jinjia2 内置过滤器](http://jinja.pocoo.org/docs/dev/templates/#builtin-filters).
[Ansible 过滤器](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html).

自定义过滤器: Ansible 会在 playbook 的同级的 filter_plugins 目录下查找自定义过滤器.


### Role
Role 是将 Playbook 分割为多个文件的主要机制.
Role 可以看成你分配给一台或多台主机的配置与操作的集合. 每个 Ansible 的 Role 都会有一个名字, 比如 database. 与 database role 相关的文件都放在 roles/database 目录中. 其结构如下:
* `roles/database/tasks/mail.yml`: task 文件.
* `roles/database/files/`: 需要上传至主机的文件.
* `roles/database/templates/`: Jinja2 模板文件.
* `roles/database/handlers/main.yml`: handler 文件.
* `roles/database/vars/main.yml`: 不应被覆盖的变量.
* `roles/database/defaults/main.yml`: 可以被覆盖的默认变量.
* `roles/database/meta/main.yml`: role 的从属信息.

当使用 role 时, 需要在 playbook 中增加一个 roles 区段. roles 区段需要列出一个所使用 role 的列表.
在 role 区段中定义的变量会覆盖在 role 目录中定义的变量.

#### pre-tasks 和 post-tasks
Ansible 把在 role 之前执行的一系列 task 定义在 `pre_tasks` 区段, 而在 role 之后执行的一系统 task 定义在 `post_tasks` 区段.

#### dependent
当定义一个 role 的时候, 可以指定它依赖一个或多个其他 role(通过 `meta/mail.yml` 的 `dependencies` 区段). Ansible 将会确保被指定为从属的 role 一定会被先执行.
Ansible 允许向从属 role 传递参数.


#### Ansible Galaxy
Ansible Galaxy 是由 Ansible 社区维护的 Ansible role 开源仓库.

##### ansible-galaxy
可以用来生成 role 目录的 scaffolding: `ansible-galaxy init --init-path=playbooks/roles web`.
可以自 Ansible Galaxy 中下载 role: `ansible-galaxy install -p playbooks/rolesbennojoy.ntp`.



## 变量
**在 Ansible 中, 变量的作用域载体为主机. 只有针对特定主机讨论变量的值才有意义**.
如果运行 ansible-playbook 时传入 `-e var=value` 参数设置的变量, 那么该变量将拥有最高优先级. 也可通过 `-e @filename.yml` 的形式将文件中的变量传入.

### lookup
lookup 函数允许你从指定来源读取配置数据并在你的 playbook 和模板中使用这些数据.
Ansible 所有的 lookup 插件都是在控制主机上执行的, 并不是在远程主机上执行的.
lookup 函数接收两个参数: lookup 的类型和传递给类型的参数. 常见类型有:
* file: 自文件中读取数据并返回.
* pipe: 调用一个外部程序并将这个程序的输出返回.
* env: 读取环境变量的值.
* password: 生成随机密码并写入到指定文件.

还有 template, csvfile, dnstxt, redis_kv, etcd 等.
你还可以编写自己的 lookup 实现.

### 循环
with_items, with_lines, with_fileglob, with_first_found, with_dict, with_flattened, with_indexed_items, with_nested, with_random_choice, with_sequence, with_subelements, with_together, with_inventory_hostnames.

在 Ansible 中, 循环结构还可用来替代 lookup 插件. 只需在 lookup 插件前添加 with 就可以在循环结构中实现 lookup 插件的功能.

迭代变量名默认为 item, 使用 loop_var 可以为其指定其他名字.
可使用 loop_control 语句为循环添加标签.

### include
include 特性允许你引用 task 甚至整个 playbook, 具体取决于你定义 include 的位置.
include 的值中可使用变量, 以实现动态引用的效果(需设置 static: no).
include_role 语句不仅允许我们选择引入 role 的哪些部分, 还可以决定引入到 play 的哪个位置.

### block
block 语句允许你一次性地对 block 内所有任务设置条件与参数.
Ansible 默认的异常处理行为是: 如果 task 失败, 那么终止并退出对失败主机的执行; 但是, 只要还有主机没有出现错误, 就会继续对其他主机执行 play.
通过结合使用 serial 和 max_fail_percentage 语句, Ansible 还向你提供了随时让 play 退出的控制能力.



### playbook
通过 vars 区段定义变量.
通过定义名为 vars_file 的区段将变量放到文件中.
调用模块时可使用 register 语句将模块的输出赋值给一个字典类型的变量.

### fact
如果 Ansible 的 fact gathering 处于启用状态, 那么 Ansible 会收集主机的各种详细信息(CPU, 操作系统等)并保存在 Ansible 变量的默认空间中.
如果我们想按照 Linux 发行版(如 Ubuntu, CentOS)来划分群组, 就可以使用 fact 中的 `ansible_distribution` 变量. 然后使用 apt 模块针对 Ubuntu 系统安装软件包, 用 yum 模块针对 CentOS 系统安装软件包.
Ansible 还允许使用 `set_fact` 模块在 task 中为 facts 中的变量设置别名.

#### 主机本地变量
可以将变量定义文件放置在目标主机的 `/etc/ansible/facts.d` 目录下, 运行 paybook 时就可通过变量 `ansible_local` 来访问他们.

### 内置变量
* hostvars: 所有的 Ansible 主机上的配置等信息集.
* inventory_hostname, inventory_hostname_short: 目标主机名.
* groups_names, groups: 组相关信息.
* ansible_check_mode: 布尔值.
* ansible_play_batch: Ansible 批量执行时的 inventory 主机名.
* ansible_play_hosts: 当前 play 涉及的 inventory 主机名.
* ansible_version: 版本.

### 优先级
1. `ansible-playbook -e var=value`
2. task 变量
3. block 变量
4. role 中定义的和 include 的变量
5. set_fact
6. registered 变量
7. vars_files
8. vars_prompt
9. play 变量
10. Host facts
11. playbook 中设置的 host_vars
12. playbook 中设置的 group_vars
13. inventory 中设置的 host_vars
14. inventory 中设置的 group_vars
15. inventory 变量
16. role 中 defaults/main.yml 中定义的变量


## Inventory
在 Ansible 中, 描述你的主机的默认方法是将它们列在一个文本文件中(ini 格式), 这个文本文件叫作 inventory 文件. 默认为 `/etc/ansible/hosts`.
Ansible 默认使用本地的 SSH 客户端,这意味着你在 SSH 配置文件中设置的任何别名都可以被识别.
Ansible 默认会将主机名 localhost 作为本机解析.

### 群组
Ansible 自动定义了一个群组叫做 all(或 `'*'`), 它包括 inventory 中所有的主机.
可以在 inventory 文件中定义自己的群组. Ansible 的 inventory 文件是 ini 格式的. 在 ini 格式中, 同类的配置值归类在一起组成区段(`[<name>]`).
群组的群组: 使用 `[<name>:children]` 格式的配置可以为多个群组定义一个统一的名字.
主机名支持使用通配的方式来定义: `web[01-16].kyo.rocks`, `web[a-z].kyo.rocks`.
可以在 inventory 文件中为主机定义变量以供 playbook 使用: `dev env=dev`.
可以在 inventory 文件中单独定义变量:

```ini
    [all:vars]
    ntp_server=ntp.ubuntu.com
    [prod:vars]
    db_host=192.168.0.2
    [staging:vars]
    db_host=127.0.0.1
```

### 分文件管理
在 inventory 文件中, 只能将变量指定为布尔型和字符串.
可以为每台主机(定义于 host_vars 目录)和每个群组(定义于 group_vars 目录)创建独立的变量文件, Ansible 会使用 Yaml 格式来解析这些变量文件.
如果使用了 Yaml 格式, 那么访问变量的方式也要使用 `.` 的形式: `db_primary_host => db.primary.host`.
如果你想更进一步地进行拆分, Ansible 还允许将主机或群组的名字定义为一个目录, 然后将变量放置在这个目录中.
通常来说, 将变量分割到太多的文件也会带来管理上的问题, **保持配置足够简单才是更好的选择**.

### 动态 Inventory
如果 Inventory 文件被标记为可执行, 那么 Ansible 会假设这是一个动态的 Inventory 脚本并执行它而不是读取它的内容.

## Vanlt
ansible-vault 命令可用于创建和编辑加密文件. ansible-playbook 可以在输入密码后自动识别并解密加密文件.
如果 `--vault-password-file` 参数所指向的文件权限被设置为可执行, Ansible 将会执行这个文件并使用它的标准输出做为密码. 这个特性允许你使用脚本向 Ansible 提供密码.

## CLI
### ansible
ansible 命令通常用作点对点, 一次性的事情.
ansible 查找 ansible.cfg 文件的顺序:
1. 环境变量 ANSIBLE_CONFIG
2. ./ansible.cfg
3. ~/.ansible.cfg
4. /etc/ansible/ansible.cfg

可以将 ansible.cfg 和 playbook 放在同一目录, 以便于使用版本控制.

### ansible-doc
用于查看模块的文档.

## Modules
模块是由 Ansible 包装的, 在主机上执行一系列操作的脚本.
Ansible 模块是声明式的. 模块声明它可以用于描述你希望服务器所处于的状态.
在 Ansible 社区, 复用的基本单元是模块.

### command
此模块为 Ansible 命令的默认模块.

#### options
* -a: 指定要执行的命令. `ansible all -a uptime`.
* -b: 使用 sudo 执行命令.

### debug
Ansible 版的 print 语句. 你可以使用它打印变量的值或者任意字符串.

### ping
测试主机是否可用. `ansible all -m ping`.

### setup
用于主机信息 fact 的收集.  


## FAQs
* `'ansible_env' is undefined`:  
    没有开启 `gather_facts` 导致的. 将 `gather_facts` 设置为 true 即可.