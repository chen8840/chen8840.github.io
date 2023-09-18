### word-break,word-wrap,line-break相关知识

<span style="color: red">1.word-break: break-word与word-wrap: break-word的区别？</span>

答：计算最小宽度（width: min-content）时有区别，word-break: break-word计算的是单个字符的宽度，word-wrap: break-word计算的是单个单词的宽度。

另：<span style="color: red">word-break:break-word</span> has the same effect as <span style="color: red">overflow-wrap:anywhere</span>.

<span style="color: red">2.word-break: break-all与word-wrap: break-word的区别？</span>

答：word-wrap 是用来决定允不允许单词内断句的，如果不允许的话长单词就会溢出。最重要的一点是它还是会首先尝试挪到下一行，看看下一行的宽度够不够，不够的话就进行单词内的断句。

而word-break:break-all则更变态，因为它断句的方式非常粗暴，它不会尝试把长单词挪到下一行，而是直接进行单词内的断句

<span style="color: red">3.word-break: break-all与word-wrap: break-word什么时候需要结合使用？</span>

答：word-break: break-all虽然能在任何地方进行断行，但是对于一些中文字符则不行，比如“——”破折号

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .p {
            background: yellow;
            width: 50px;
            word-break: break-all;
            overflow: hidden;
            resize: horizontal;
        }
    </style>
</head>
<body>
    <div class="p">
        h12 h1234 hfdaga ————————————————————
    </div>
</body>
</html>
```

上面的破折号就会冲到容器的外面去，这时需要word-wrap: break-word的能力，将这些特殊字符进行断行。

<span style="color: red">4.line-break:anywhere的作用？</span>

答：即使同时使用了word-break: break-all和word-wrap: break-word，HTML任会遵守一些排版规则，比如中文句号“。”不允许出现在行首。

如果想完全规避这些规则，就使用line-break:anywhere。它是真正意义上的随处断行。
