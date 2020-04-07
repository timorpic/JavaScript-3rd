# 2.1 &lt;script&gt; 元素

向 HTML 页面中插入 JavaScript 的主要方法，就是使用  元素。这个元素由 Netscape 创造 并在 Netscape Navigator 2中首先实现。后来，这个元素被加入到正式的 HTML 规范中。HTML 4.01 为定义了下列 6 个属性。 

* async ：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作，比如下载其他资源或 等待加载其他脚本。只对外部脚本文件有效。 
* charset ：可选。表示通过 src 属性指定的代码的字符集。由于大多数浏览器会忽略它的值， 因此这个属性很少有人用。 
* defer ：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有 效。IE7 及更早版本对嵌入脚本也支持这个属性。 
* language ：已废弃。原来用于表示编写代码使用的脚本语言（如 JavaScript 、 JavaScript1.2 或 VBScript ）。大多数浏览器会忽略这个属性，因此也没有必要再用了。 
* src ：可选。表示包含要执行代码的外部文件。 
* type ：可选。可以看成是 language 的替代属性；表示编写代码使用的脚本语言的内容类型（也 称为 MIME 类型）。虽然 text/javascript和text/ecmascript 都已经不被推荐使用，但人 们一直以来使用的都还是 text/javascript 。实际上，服务器在传送 JavaScript 文件时使用的 MIME 类型通常是 application/x–javascript ，但在 type 中设置这个值却可能导致脚本被 忽略。另外，在非IE浏览器中还可以使用以下值： application/javascript 和 application/ecmascript 。考虑到约定俗成和最大限度的浏览器兼容性，目前 type 属性的值依旧还是 text/javascript 。不过，这个属性并不是必需的，如果没有指定这个属性，则其默认值仍为 text/javascript 。

使用&lt;Script&gt;元素的方式有两种：直接在页面中嵌入 JavaScript 代码和包含外部 JavaScript 文件。 

在使用&lt;Script&gt;元素嵌入 JavaScript 代码时，只须为  指定 type 属性。然后，像下面这 样把 JavaScript 代码直接放在元素内部即可：

```text
<script type="text/javascript">
    function sayHi(){
        alert("Hi!");
    }
</script>
```

包含在&lt;script&gt;元素内部的 JavaScript 代码将被从上至下依次解释。就拿前面这个例子来说，解释 器会解释一个函数的定义，然后将该定义保存在自己的环境当中。在解释器对  元素内部的所 有代码求值完毕以前，页面中的其余内容都不会被浏览器加载或显示。

在使用&lt;script&gt;嵌入 JavaScript 代码时，记住不要在代码中的任何地方出现 "&lt;/script&gt;" 字符串。 例如，浏览器在加载下面所示的代码时就会产生一个错误：

```text
<script type="text/javascript">
    function sayScript(){
        alert("</script>");
    }
</script>
```

因为按照解析嵌入式代码的规则，当浏览器遇到字符串 "&lt;/script&gt;" 时，就会认为那是结束的 &lt;/script&gt; 标签。而通过转义字符“/”可以解决这个问题，例如：

```text
<script type="text/javascript">
    function sayScript(){
        alert("<\/script>");
    }
</script>
```

这样写代码浏览器可以接受，因而也就不会导致错误了。

 如果要通过  元素来包含外部 JavaScript 文件，那么 src 属性就是必需的。这个属性的值 是一个指向外部 JavaScript 文件的链接，例如：

```text
<script type="text/javascript" src="example.js"></script>
```

在这个例子中，外部文件 example.js 将被加载到当前页面中。外部文件只须包含通常要放在开始的&lt;script&gt;和结束的&lt;/script&gt;之间的那些 JavaScript 代码即可。与解析嵌入式 JavaScript 代码一样， 在解析外部 JavaScript 文件（包括下载该文件）时，页面的处理也会暂时停止。如果是在 XHTML 文档 中，也可以省略前面示例代码中结束的 &lt;/script&gt; 标签，例如：

```text
<script type="text/javascript" src="example.js" />
```

但是，不能在 HTML 文档使用这种语法。原因是这种语法不符合 HTML 规范，而且也得不到某些 浏览器（尤其是 IE）的正确解析。

> 按照惯例，外部 JavaScript 文件带有.js扩展名。但这个扩展名不是必需的，因为 浏览器不会检查包含 JavaScript 的文件的扩展名。这样一来，使用 JSP、PHP 或其他 服务器端语言动态生成 JavaScript 代码也就成为了可能。但是，服务器通常还是需要 看扩展名决定为响应应用哪种 MIME 类型。如果不使用.js 扩展名，请确保服务器能 返回正确的 MIME 类型。

需要注意的是，带有 src 属性的&lt;script&gt;元素不应该在其&lt;script&gt;和&lt;/script&gt;标签之间再包含额外的 JavaScript 代码。如果包含了嵌入的代码，则只会下载并执行外部脚本文件，嵌入的代码会被忽略。 

另外，通过&lt;script&gt;元素的 src 属性还可以包含来自外部域的 JavaScript 文件。这一点既让&lt;script&gt; 元素倍显强大，又让它备受争议。在这一点上，  与&lt;script&gt;元素非常相似，即它的 src 属性可以是指向当前 HTML 页面所在域之外的某个域中的完整 URL，例如：

```text
<script type="text/javascript" src="http://www.somewhere.com/afile.js"></script>
```

 这样，位于外部域中的代码也会被加载和解析，就像这些代码位于加载它们的页面中一样。利用这 一点就可以在必要时通过不同的域来提供 JavaScript 文件。不过，在访问自己不能控制的服务器上的 JavaScript 文件时则要多加小心。如果不幸遇到了怀有恶意的程序员，那他们随时都可能替换该文件中 的代码。因此，如果想包含来自不同域的代码，则要么你是那个域的所有者，要么那个域的所有者值得 信赖。 

无论如何包含代码，只要不存在 defer 和 async 属性，浏览器都会按照  元素在页面中 出现的先后顺序对它们依次进行解析。换句话说，在第一个  元素包含的代码解析完成后，第 二个  包含的代码才会被解析，然后才是第三个、第四个……

