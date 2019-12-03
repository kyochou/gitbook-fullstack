# Perl

## CLI
### FAQs
* 正则中使用 Shell 中的变量:

    ```shell
    # 使用 Env 模块和正则转义 \Q$variable\E
    name=kyo perl -MEnv -i'' -pe's/joe/\Q$name\E/g' filename
    ```