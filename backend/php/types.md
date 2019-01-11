# Types

## Strings

### FAQs
* heredoc 中调用函数

    ```php
$k_f = function ($fn) {
    return $fn;
};
echo <<<HEREDOC
Hello, {$k_f(ucfirst('world'))}
HEREDOC;
    ```