# ```php://filter```的原理和利用研究

## 原理

```php://filter```是```PHP```语言中特有的协议流，作用是作为一个“中间流”来处理其他流。

比如，我们可以用如下一行代码将```POST```内容转换成```base64```编码并输出：

```php
readfile("php://filter/read=convert.base64-encode/resource=php://input");
```

结果如下：

![image](https://www.leavesongs.com/content/uploadfile/201607/0f851469385893.png)

```php://filter```使用不同的参数可以达到不同的目的和效果：

| **名称**                    | **描述**                                                     | **备注** |
| --------------------------- | ------------------------------------------------------------ | -------- |
| `resource=<要过滤的数据流>` | 指定要筛选过滤的数据流                                       | 必须     |
| `read=<读链的筛选列表>`     | 设定一个或多个过滤器名称，以管道符（`\|`）分隔                | 可选     |
| `write=<写链的筛选列表>`    | 设定一个或多个过滤器名称，以管道符（`\|`）分隔                | 可选     |
| `<；两个链的筛选列表>`      | 任何没有以 `read=` 或 `write=` 作前缀 的筛选器列表会视情况应用于读或写链 |          |

可用的过滤器列表如下所示：

| 名称         | 类型                                  | 说明                                                         |
| ------------ | ------------------------------------- | ------------------------------------------------------------ |
| 字符串过滤器 | ```string.rot13```                    | 对字符串执行 ```ROT13(使用字母表中后面第 13 个字母替换当前字母，同时忽略非字母表中的字符) ```转换 |
|              | ```string.toupper```                  | 将字符串转化为大写                                           |
|              | ```string.tolower```                  | 将字符串转化为小写                                           |
|              | ```string.strip_tags```               | 从字符串中去除 `HTML`和 `PHP` 标记（注意：`PHP 7.3.0 `起已废弃该特性） |
| 转换过滤器   | ```convert.base64-encode```           | ```base64```编码                                             |
|              | ```convert.base64-decode```           | ```base64```解码                                             |
|              | ```convert.quoted-printable-encode``` | 将 ```8-bit ```字符串转换为 ```quoted-printable ```字符串    |
|              | `convert.quoted-printable-decode`     | 将 ```quoted-printable``` 字符串转换为 ```8-bit``` 字符串    |
| 压缩过滤器   | `zlib.deflate`                        | 压缩                                                         |
|              | `zlib.inflate`                        | 解压                                                         |
|              | `bzip2.compress`                      | 压缩                                                         |
|              | `bzip2.decompress`                    | 解压                                                         |
| 加密过滤器   | `mcrypt.*`                            | 加密，`*`表示加密算法                                        |
|              | `mdecrypt.*`                          | 解密，`*`表示解密算法                                        |



## 利用思路

### 读取源码文件

`php://filter`之前最常出镜的地方是`XXE`。由于`XXE`漏洞的特殊性，我们在读取`HTML`、`PHP`等文件时可能会抛出此类错误`parser error : StartTag: invalid element name` 。其原因是，`PHP`是基于标签的脚本语言，`<?php ... ?>`这个语法也与`XML`相符合，所以在解析XML的时候会被误认为是`XML`，而其中内容（比如特殊字符）又有可能和标准`XML`冲突，所以导致了出错。

那么，为了读取包含有敏感信息的`PHP`等源文件，我们就要先将“可能引发冲突的`PHP`代码”编码一遍，这里就会用到`php://filter`。

在`XXE`中，我们可以将`PHP`等容易引发冲突的文件流用`php://filter`协议流处理一遍，这样就能有效规避特殊字符造成混乱。

![](https://www.leavesongs.com/content/uploadfile/201607/693b1469385893.png)

### 通过写编码实行绕过操作写入```webshell```

以下面例子作为讲解：

```php
<?php
$content = '<?php exit; ?>';
$content .= $_POST['txt'];
file_put_contents($_POST['filename'], $content);
```

`$content`在开头增加了exit过程，导致即使我们成功写入一句话，也执行不了（这个过程在实战中十分常见，通常出现在缓存、配置文件等等地方，不允许用户直接访问的文件，都会被加上```if (!defined(xxx)) exit;```之类的限制）。

那么在这种情况下，如何绕过这个“死亡```exit```"？

#### 思路一：巧用编码与解码

幸运的是，这里的`$_POST['filename']`是可以控制协议的，我们即可使用 ```php://filter```协议来施展魔法：使用```php://filter```流的```base64-decode```方法，将`$content`解码，利用```php```的```base64_decode```函数特性去除“死亡```exit```”。

众所周知，```base64```编码中只包含64个可打印字符，而```PHP```在解码```base64```时，遇到不在其中的字符时，将会跳过这些字符，仅将合法字符组成一个新的字符串进行解码。

所以，当`$content`被加上了`<?php exit; ?>`以后，我们可以使用 ```php://filter/write=convert.base64-decode ```来首先对其解码。在解码的过程中，字符```<```、```?```、```;```、```>```、空格等一共7个字符不符合```base64```编码的字符范围将被忽略，所以最终被解码的字符仅有```phpexit```和我们传入的其他字符。

```phpexit```一共7个字符，因为```base64```算法解码时是4个```byte```一组，因此需要另外增加1个字符一共8个字符以让```base64```顺利解码（本例中新增```a```字符）。这样，```phpexita```被正常解码，而后面我们传入的```webshell```的```base64```内容也被正常解码。结果就是`<?php exit; ?>`没有了。

![image](https://www.leavesongs.com/content/uploadfile/201607/fca81469385894.png)

#### 思路二：利用字符串操作方法

我们观察一下，这个`<?php exit; ?>`实际上是什么？

实际上是一个```XML```标签，既然是```XML```标签，我们就可以利用```strip_tags```函数去除它，而```php://filter```刚好是支持这个方法的。

我们最终的目的是写入一个```webshell```，而写入的```webshell```也是```php```代码，如果使用```strip_tags```同样会被去除。

万幸的是，```php://filter```允许使用多个过滤器，我们可以先将```webshell```用```base64```编码。在调用完成```strip_tags```后再进行```base64-decode```。“死亡```exit```”在第一步被去除，而```webshell```在第二步被还原。

![](https://www.leavesongs.com/content/uploadfile/201607/95b61469385895.png)

除此之外，我们还可以利用```rot13```编码独立完成任务。原理和上面类似，核心是将“死亡```exit```”去除。`<?php exit; ?>`在经过```rot13```编码后会变成`<?cuc rkvg; ?>`，在```PHP```不开启```short_open_tag```时，```php```不认识这个字符串，当然也就不会执行了。

![](https://www.leavesongs.com/content/uploadfile/201607/1c471469385896.png)
