# Mobile

## 表单
### FAQs
* 显示数字键盘且输入数据为文本形式(如验证码, 直接设置 type 值为 number 会导致输入值的前导 0 被自动移除). 参见 [Controlling which iOS keyboard is shown](http://sja.co.uk/controlling-which-ios-keyboard-is-shown).

    ```html
    <input type="text" pattern="[0-9]*" class="form-control" id="captcha" ng-model="login.captcha" placeholder="验证码" required>
    ```