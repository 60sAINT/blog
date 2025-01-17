---
title: Hexo 中 Markdown 特殊字符的处理方法
cover: assets/md.png
tags:
  - Hexo
  - Markdown
---

# 问题描述

在 [Hexo](https://so.csdn.net/so/search?q=Hexo&spm=1001.2101.3001.7020) 搭建的博客中对文章进行编写，经常会用到一些特殊字符需要转译，比如 `-`、`.`、空格、制表符等等，在正常情况下可以使用 `\` 进行转译，但是有一些字符无法转译，使用后在执行 `hexo server` 命令的时候会报错。

**报错信息：**

```bash
Unhandled rejection Template render error: (unknown path) [Line 7, Column 23]
Error: Unable to call `worldcount`, which is undefined or falsey......
```

# 解决方案

报错的原因是，Hexo 编译时发生错误，可能是文章中存在特殊字符，如：{ [ ( ) ] } 等等。如下面这段代码：

**在页面中：**

```
{{ worldcount(post.content) }}
```

**在 Markdown 中：**

```markdown
&#123;&#123; worldcount&#40;post.content&#41; &#125;&#125;
```

在 Markdown 中使用 `\` 无法转译的字符需要使用字符的命名实体或十进制编码，如上面代码中。

***注意：需要转义的字符只是文本中的特殊字符，代码块中的特殊字符无需转译或使用转译字符。***

# 常见特殊字符

常用特殊字符转译字符对照表：

| 特殊符号 | 命名实体  | 十进制编码 |
| :------: | :-------: | :--------: |
|   空格   | `&nbsp;`  |  `&#160;`  |
| 全角空格 | `&emsp;`  | `&#12288;` |
|   回车   |     —     |  `&#13;`   |
|    ’     | `&apos;`  |  `&#39;`   |
|    "     | `&quot;`  |  `&#34;`   |
|    (     |     —     |  `&#40;`   |
|    )     |     —     |  `&#41;`   |
|    <     |  `&lt;`   |  `&#60;`   |
|    \>    |  `&gt;`   |  `&#62;`   |
|    \[    |     —     |  `&#91;`   |
|    \]    |     —     |  `&#93;`   |
|    {     |     —     |  `&#123;`  |
|    }     |     —     |  `&#125;`  |
|    ´     | `&acute;` |  `&#180;`  |
|    °     |  `&deg;`  |  `&#176;`  |
|    ®     |  `&reg;`  |  `&#174;`  |
|    ©     | `&copy;`  |  `&#169;`  |

常用数学转译字符对照表：

| 特殊符号 |  命名实体  | 十进制编码 |
| :------: | :--------: | :--------: |
|    ≤     |   `&le;`   | `&#8804;`  |
|    ≥     |   `&ge;`   | `&#8805;`  |
|    ≈     | `&asymp;`  | `&#8773;`  |
|    ≠     |   `&ne;`   | `&#8800;`  |
|    ∩     |  `&cap;`   | `&#8745;`  |
|    ∪     |  `&cup;`   | `&#8746;`  |
|    ∠     |  `&ang;`   | `&#8736;`  |
|    ∞     | `&infin;`  | `&#8734;`  |
|    ±     | `&plusmn;` |  `&#177;`  |
|    √     | `&radic;`  | `&#8730;`  |
|    ∑     |  `&sum;`   | `&#8722;`  |
|    ∫     |  `&int;`   | `&#8747;`  |
|    Δ     | `&Delta;`  |  `&#916;`  |

常用希腊字母转译字符对照表：

| 特殊符号 |  命名实体   | 十进制编码 |
| :------: | :---------: | :--------: |
|    Φ     |   `&Phi;`   |  `&#934;`  |
|    Ω     |  `&Omega;`  |  `&#937;`  |
|    α     |  `&alpha;`  |  `&#945;`  |
|    β     |  `&beta;`   |  `&#946;`  |
|    γ     |  `&gamma;`  |  `&#947;`  |
|    δ     |  `&delta;`  |  `&#948;`  |
|    ε     | `&epsilon;` |  `&#949;`  |
|    ζ     |  `&zeta;`   |  `&#950;`  |
|    η     |   `&eta;`   |  `&#951;`  |
|    θ     |  `&theta;`  |  `&#952;`  |
|    λ     | `&lambda;`  |  `&#955;`  |
|    μ     |   `&mu;`    |  `&#956;`  |
|    ξ     |   `&xi;`    |  `&#958;`  |
|    π     |   `&pi;`    |  `&#960;`  |
|    ρ     |   `&rho;`   |  `&#961;`  |
|    σ     |  `&sigma;`  |  `&#963;`  |
|    φ     |   `&phi;`   |  `&#966;`  |
|    ψ     |   `&psi;`   |  `&#968;`  |
|    ω     |  `&omega;`  |  `&#969;`  |
|    ∂     |  `&part;`   | `&#8706;`  |
|    ∅     |  `&empty;`  | `&#8709;`  |