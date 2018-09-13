# 基本编码规范

本规范包含了基本的代码规范标准，从而能够在共享的PHP代码间保持高度的互通性。

本规范中的 "必须", "不得", "需要", "应", "不应", "应该",
"不应该", "推荐", "可能", 和 "可选" 在 [RFC 2119]中有具体的解释

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md

## 1. 概览

- 文件 **必须** 只使用 `<?php` 或者 `<?=` 标签;

- 文件 **必须** 只使用不带BOM头的UTF-8编码;

- 文件 **应该** *或者* 只声明或定义类, 函数, 常量等 *或者* 只产生 *副作用* 例如，生成文件输出， 改变`.ini`配置等，但是不可以同时作用;

- 命名空间和类必须遵循PSR的自动加载规范，[[PSR-0]或者[PSR-4]](推荐);

- 类的命名必须遵循大写开头的驼峰命名规范;

- 类的常量的命名必须遵循大写规范，并且单词之间以下划线分隔;

- 类的方法的命名必须遵循小写开头的驼峰命名规范;

## 2. 文件

### 2.1. PHP标签

PHP代码必须使用`<?php ?>`长标签或者`<?= ?>`短标签，不得使用其他标签

### 2.2. 字符集编码

PHP代码必须使用不带BOM头的UTF-8编码

### 2.3. 副作用

PHP文件应该包含声明或者定义的新的类，函数或者常量等，并且不产生副作用，或者包含只执行产生副作用的逻辑操作，但是不可以同时包含这两种内容。

副作用的意思是仅仅通过引入文件
The phrase "side effects" means execution of logic not directly related to
declaring classes, functions, constants, etc., *merely from including the
file*.

"Side effects" include but are not limited to: generating output, explicit
use of `require` or `include`, connecting to external services, modifying ini
settings, emitting errors or exceptions, modifying global or static variables,
reading from or writing to a file, and so on.

The following is an example of a file with both declarations and side effects;
i.e, an example of what to avoid:

~~~php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
~~~

The following example is of a file that contains declarations without side
effects; i.e., an example of what to emulate:

~~~php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
~~~

## 3. Namespace and Class Names

Namespaces and classes MUST follow an "autoloading" PSR: [[PSR-0], [PSR-4]].

This means each class is in a file by itself, and is in a namespace of at
least one level: a top-level vendor name.

Class names MUST be declared in `StudlyCaps`.

Code written for PHP 5.3 and after MUST use formal namespaces.

For example:

~~~php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
~~~

Code written for 5.2.x and before SHOULD use the pseudo-namespacing convention
of `Vendor_` prefixes on class names.

~~~php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
~~~

## 4. Class Constants, Properties, and Methods

The term "class" refers to all classes, interfaces, and traits.

### 4.1. Constants

Class constants MUST be declared in all upper case with underscore separators.
For example:

~~~php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
~~~

### 4.2. Properties

This guide intentionally avoids any recommendation regarding the use of
`$StudlyCaps`, `$camelCase`, or `$under_score` property names.

Whatever naming convention is used SHOULD be applied consistently within a
reasonable scope. That scope may be vendor-level, package-level, class-level,
or method-level.

### 4.3. Methods

Method names MUST be declared in `camelCase()`.
