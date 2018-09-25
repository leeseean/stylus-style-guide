Stylus编码规范
============================

*编写健壮stylus代码指南*

Table of Contents
-----------------
    
1. [基本准则](#general)
2. [格式化](#formatting)
    - [空格和缩进](#spacing)
    - [属性书写顺序](#sorting)
    - [连写](#chaining)
    - [嵌套](#nesting)
    - [简写属性](#properties)
    - [简写值](#values)
    - [引号](#quotes)
3. [命名](#naming)
    - [基础](#case)
    - [id和class选择器](#ids)
    - [分隔符](#delimiters)
4. [Mixins](#mixins)
5. [Fonts](#fonts)
6. [变量](#variables)
    - [定义](#declaration)
    - [默认值](#defaults)
    - [计算](#manipulation)
7. [注释](#comments)
8. [Thanks](#thanks)



<a name="general">基本准则</a>
-----------

确保选择器和注释旁边有足够的空间，对齐很重要，确保选择器和属性之间的缩进。需要引入的文件需要在文件名前面加一个`_`来确保更容易找到文件。

**Be consistent.** 如果你在编写代码，花几分钟检查你的代码风格并确保风格一致。如果所有算术运算符周围使用空格，你也应该这样做。 如果注释周围几乎没有哈希标记，那么你的注释周围也不应该有哈希标记。

统一编码风格是为了方便代码阅读与维护。



<a name="formatting">格式化</a>
----------


### <a name="spacing">空格与缩进</a>

缩进已“4个空格”为准，删除不必要的尾随空格。

```sass
// bad
h1
  padding: 0
  margin: 0
h2 
  color: #846383
  margin-bottom: 20px



// good
h1 {
    margin: 0
    padding: 0
}
    
h2 {
    color: #846383
    margin-bottom: 20px
}
```

### <a name="sorting">属性书写顺序</a>

每个属性都应该单独写在一行，属性书写顺序按照属性首字母alpha字母表排序，方法和include应该写在最前面并且也以alpha字母表排序。


```sass
// bad
h1
    padding: 5px
    clear()
    width: 200px
    color: #000


// good
h1 {
    clear()
    color: #000
    padding: 5px
    width: 200px
}
```

### <a name="chaining">连写</a>

必要时，如果有很多选择器拥有同样的样式，则可以把这些选择器写在一起并以“,”隔开。

```sass
// bad
ol
    color: #000
    line-height: 1.5
    list-style: none outside none
    padding: 0

ul
    color: #666
    line-height: 1.5
    list-style: none outside none
    padding: 0


// good
ol, ul {
    color: #000
    line-height: 1.5
    list-style: none outside none
    padding: 0
}
    
ul {
    color: #666
}
```

### <a name="nesting">嵌套</a>

选择器嵌套只在有必要时用，由于使用它们带来的性能损失，也应该谨慎使用直接后代。 当发生大量嵌套时，嵌套会导致文件体积过大。

后代选择器是低效的，因为对于与键匹配的每个元素，浏览器还必须遍历DOM树，计算每个祖先元素，直到找到匹配或到达根元素。 键越不具体，需要计算的节点数就越多。

嵌套应该是四个空格，任何选择器都应该在它们之前有一个换行符。

```sass
// bad
body
    border-top: 1px solid #000
    
    > article
        background: #ccc
        padding: 0
        
    h1
        color: #000


// good
body {
    border-top: 1px solid #000
}
    
article {
    background: #ccc
    padding: 0
}
    
h1 {
    color: #000
}
```

类，伪元素，伪类和其他修饰符应嵌套在其选择器中以保持可读性。 具有上述换行符的相同规则适用于这些修饰符。 伪类修饰符应尽可能靠近其父选择器。 这允许开发人员快速查看任何悬停或其他状态，而无需在文件中稍后找到它们。 类修饰符也应该尽可能靠近它们的选择器。

```sass
// bad
h1.green
    color: #0085e3
        
h1:first-child
    padding-top: 0


// good
h1 {
    &:first-child {
        padding-top: 0
    }

    &.green {
        color: #0085e3
    }
}

```

元素和非常常见的类的全局样式应放在全局样式表的顶部。 这可以包括body, link, input, paragraph, and header样式。

```sass
@charset "UTF-8"

body {
    border-top: 3px solid #333
}

a {
    color: #008fc5
    cursor: pointer
    outline: none
    text-decoration: none
}

input {
    border: 1px solid #ccc
}
    
h1 {
    font-size: font-size + 10
}
    
p {
    color: #666
    line-height: 1.5
}
    
        
// rest of the file
```
    



### <a name="properties">简写属性</a>


CSS提供了各种速记属性（如`font`，`background，`list-style`等），这些属性应在第一次声明时使用。

**Font**

`font`应该在body标签上声明，占用单行如下用法：

```sass
font: font-style font-weight font-variant font-size/line-height font-family
// font: normal normal normal 14px/1.5 Arial, Helvetica, sans-serif
```

对该字体标记的任何后续修改都应该在多行声明中完成，*除非*对`font-family`进行了更改，否则需要完整的声明。 这将保留文档的单一字体并促进正确的级联。

`font-weight`应始终使用数字而不是关键字来声明。 这有助于使用`@ font-face`声明。 `line-height`应始终使用`em`声明，但不使用单位。


**Background**

`background`首次使用应使用单行声明。

```sass
background: color image repeat attachment position(left, top) size clip attachment
// background: #eee url("img/pic.jpg") no-repeat scroll left top auto padding-box border-box 
```

像`font`一样，对该字体标记的任何后续修改都应该在多行声明中完成，*除非*对`url（）`进行了更改，否则需要完整的声明。 一个例外是如果原始声明只设置一种颜色，那么单个`background：color`简写是可以接受的。
```sass
background: #fff
```

**List Style**

与前面两个属性一样，`list-style`应该在第一次使用单行声明时声明。 对该字体标记的任何后续修改都应该在多行声明中完成，*除非*对`url（）`进行了更改，否则需要完整声明。

```sass
list-style: list-style-type list-style-position list-style-image
// list-style: disc outside none
```


### <a name="values">简写值</a>

除非需要，否则在“0”值之后省略单位规范。 对于允许它的颜色值，3个字符的十六进制表示法更短，更简洁。 作为一般规则，如果不需要单元规范，则不要使用它。

```sass
// bad
p
    color: #000000
    padding: 0px
    line-height: 1.5em


// good
p {
    color: #000
    padding: 0
    line-height: 1.5
}
```



简写值可以用在 `padding`, `margin`, 和 `border` 如果需要设置多个值。

```sass
// bad
p
    padding-left: 20px
    padding-top: 15px
    

// good
p {
    padding: 15px 0 0 20px
}
    
```

对于`padding`, `margin`, and `border`，1、2、4个值可以接受的，不建议写3个值，以为这不容易理解。

```sass
// bad
p
    padding: 0 20px 10px


// good
p {
    padding: 0
}
    
q {
    padding: 0 20px
}

time {
    padding: 10px 20px 15px 5px
}

```

同样地，当声明`box-shadow`和`text-shadow`时，不声明一些值是不好的，始终声明这些属性的所有值。

```sass
box-shadow: inset 3px 3px 1px 0 rgba(0,0,0,0.3)
// box-shadow: none h-shadow v-shadow blur spread color 

text-shadow: 1px 1px 1px rgba(0,0,0,0.5)
//text-shadow: h-shadow v-shadow blur color
```


### <a name="quotes">引号</a>

使用双引号`"`，不使用单引号`'`，

```sass
// bad
html
    background: #fff url('img/bg.png') repeat 0 0
    font-family: 'open sans', arial, sans-serif

input[type=search]
    margin-left: 1em


// good
html {
  background: #fff url("img/bg.png") repeat 0 0
  font-family: "open sans", arial, sans-serif
}

input[type="search"] {
  margin-left: 1em
}
```






<a name="naming">命名</a>
------


### <a name="case">基础</a>

**只使用小写命名**。除了一些属性值如`content`外都使用小写。

```sass
// bad
UL
    color: #EEEEEE
    line-height: 1.5
    
    &:FIRST-CHILD
        padding-top: 20px


// good
ul {
    color: #000
    line-height: 1.5
    
    &:first-child {
        padding-top: 20px
    }
}
```




### <a name="ids">id和class选择器</a>

使用有意义或通用的ID和类名。而不是神秘难懂的名称，总是使用反映相关元素的目的的ID和类名，或者通用的。应该首选具有特定名称并反映元素用途的名称，因为这些名称最容易理解且最不可能改变。通用名称只是与其兄弟姐妹没有特定或没有任何意义的元素的后备。他们通常需要作为“助手”。 使用功能或通用名称可降低不必要的文档或模板更改的可能性。

```sass
// bad

// meaningless
#yee-1901

// presentational
.button-green, .clear


// good

// specific
.gallery, .login, .video 

// generic
.aux, .alt
```

尽可能使用短的ID和类名称。尝试尽可能简短地传达ID或类的内容。合适的ID和类名称可以方便理解以及提升编码效率。 除非必要，否则不要将元素名称与ID结合使用。

```sass
// bad

//too long
#navigation

// not descriptive enough
.atr


// good

#nav, .author
```

### <a name="delimiters">分隔符</a>

用连字符分隔ID和类名中的单词。 除了连字符之外，不要通过任何字符（包括根本没有）连接选择器中的单词和缩写，以便提高理解性。


```sass
// bad

// not readble
#videoid

// camel instead of hypen
.errorMessage


// good

#video-id, .error-message
```

<a name="mixins">Mixins</a>
------

Mixins使用`function`类型声明声明。 Mixins应该放在一个名为`_mixins.styl`的文件中。 Mixins用于经常通过样式表重复的代码，但可能不一定对应于单个元素或类。

* Mixin names should follow dashed, lowercase formatting.
* Mixins should be order alphabetically by their title for organization.
* If applicable, provide default settings in the mixin.


```sass
// bad
button(color)
    background: color
    display: block
    text-align: center


// good
button(color = blue) {
    background: color
    display: block
    text-align: center
}
```

Mixins are encouraged for multiple vendor prefix properties such as box-shadow, transition, transform, gradient, etc. 

<a name="fonts">Fonts</a>
---

如果需要外部字体文件，则应创建一个单独的字体文件并命名为`_fonts.styl`。 应使用`@ font-face`设置多种样式和权重的字体，但是应该都具有相同的字体系列名称。 确保在声明字体时提供备份字体样式。

```sass
// proxima nova
@font-face
    font-family: 'proxima-nova'
    src: url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.eot')
    src: url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.eot?#iefix') format('embedded-opentype'), url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.svg#proxima_nova_rgregular') format('svg'), url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.woff') format('woff'), url('/fonts/ProximaNovaRegular/ProximaNova-Reg-webfont.ttf') format('truetype')
    font-weight: normal
    font-style: normal
    
font-family: 'proxima-nova'
    src: url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.eot')
    src: url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.eot?#iefix') format('embedded-opentype'), url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.svg#proxima_nova_rgbold') format('svg'), url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.woff') format('woff'), url('/fonts/ProximaNovaBold/ProximaNova-Bold-webfont.ttf') format('truetype')
    font-weight: 700
    font-style: normal
```


<a name="variables">变量</a>
---------


### <a name="declaration">定义</a>

使用`"="`声明变量。变量以`"$"`开头，变量应该放在一个名为`_variables.styl`的文件中。变量用于整个样式表中使用的常用颜色，字体和数字。

* Variables should follow dashed formatting
* Ensure that variables are not ambiguous and describe the value they hold. 
* Like variables should be grouped together to help with context and readability.
* Comment each variable or group to document the values. 
* Inheritance of colors should be used whenever possible

```sass
// bad

red = #ea5b54
green = #98fe98
primary = #00853e
secondary = #008fc5

// good

// error and success colors
$error-red = #ea5b54
$success-green = #98fe98

// brand colors
$brand-primary-color = #00853e
$brand-secondary-color = #008fc5
```

### <a name="defaults">默认值</a>

每个项目都应该有与之关联的默认变量。其中包括：`font-size`，`font-family`，`line-height`。应将这些值设置为全局样式表中的正文。

```sass
body {
    font: normal $font-size/$line-height "proxima-nova", sans-serif
}
```

### <a name="manipulation">计算</a>

在样式表中计算大小调整变量时，请使用相对的`*`号而不是固定值。这允许将来进行简单的全局字体更改，以及响应式设计的简单字体缩放。任何算术运算符都应该在前后有空格。

```sass
// bad
p
    font-size: 16px
    line-height: 1.7


// good
p {
    font-size: $font-size * 1.15
    line-height: $line-height * 1.2
}
```


<a name="comments">注释</a>
--------
注释良好的代码非常重要。花时间描述组件，它们如何工作，它们的局限性以及它们的构造方式。不要让团队中的其他人猜测不常见或不明显的代码的目的。

注释样式应该在单个代码库中简单且一致。

* Place comments on a new line directly above their subject.
* Comments should have a space directly after the `//`.
* Align comments with the selectors they pertain to.
* Use terse comments that convey ideas.
* Comment major code ideas.
* Comments on every selector is unnecessary.

```sass
// bad

// too far away

h1
    color: #000
    

/*
    Improper use
    of the multiline
*/

section
    padding: 0


// good

// basic comment
h1 {
    color: #000
}

// Block of comments that
// pertain to the section
// below and other things
// and maintain spacing
section {
    padding: 0
}
```


<a name="thanks">Thanks</a>
======

Thanks to [CSS/Sass style guide](https://github.com/isellsoap/css-sass-style-guide) for an outline of ideas and veribage for expressing concepts. 
