# IDE
## IDEA
### FAQs
* 自动加载修改的类(热部署).
    * 在 `File -> Settings -> Build, Execution, Deployment -> Compiler` 下开启 "Build project automatically".
    * 打开 `Registry` 将 "compiler.automake.allow.when.app.running" 开启.
    * 打开 `Run -> Edit Configurations`, 将 "Spring Boot" 中 "On Update action" 的值设置为 "Update classes and resources". 设置后修改文件后并不会立即生效, 需要手动触发 "Update application"(默认快捷键 `CMD + F10`) 才能生效, 可以将此快捷键修改为 `CMD + S`, 完全契合了日常的开发习惯.
    * 试用了下 JRebel, 并不比 IDEA 的热加载快.
    * Refs
        * [Intellij IDEA 下 Spring Boot 热切换配置](https://segmentfault.com/a/1190000015930347)

## Eclipse
### FAQs
* 不显示自动生成代码中的 Warning.
    * 将 `Project properties > Java Build Path > Compiler > Source -> Ignore optional compile problems` 设置为 Yes 即可. 参见 [How to suppress Java warnings for specific directories or files such as generated code](https://stackoverflow.com/questions/1127920/how-to-suppress-java-warnings-for-specific-directories-or-files-such-as-generate#answer-9835628).
    