文字格式
---------------

The :ref: `screen`'s 的 ``draw.text()`` 方法可以指定文本的位置和格式。可以指定四个角
或者中心点的坐标以及各个边中点的坐标，以及单独指定中心点以及各个角。还可以设置文本的
颜色、字体、字体大小，不过字体文件要保存在当前 python 文件所在目录的 fonts 目录中，
才可以正常的设置字体。示例如下 ::

    screen.draw.text("Text color", (50, 30), color="orange")
    screen.draw.text("Font name and size", (20, 100), fontname="Boogaloo", fontsize=60)
    screen.draw.text("Positioned text", topright=(840, 20))
    screen.draw.text("Allow me to demonstrate wrapped text.", (90, 210), width=180, lineheight=1.5)
    screen.draw.text("Outlined text", (400, 70), owidth=1.5, ocolor=(255,255,0), color=(0,0,0))
    screen.draw.text("Drop shadow", (640, 110), shadow=(2,2), scolor="#202020")
    screen.draw.text("Color gradient", (540, 170), color="red", gcolor="purple")
    screen.draw.text("Transparency", (700, 240), alpha=0.1)
    screen.draw.text("Vertical text", midleft=(40, 440), angle=90)
    screen.draw.text("All together now:\nCombining the above options",
        midbottom=(427,460), width=360, fontname="Boogaloo", fontsize=48,
        color="#AAFF00", gcolor="#66AA00", owidth=1.5, ocolor="black", alpha=0.8)

``screen.draw.text`` 最简单的用法只需要提供需要显示的文本以及文本的坐标，不过文本
坐标的方式除了指定左上角的坐标，还有很多其他的方式。除了通过第二个参数指定文本的位置，
还可以通过关键字参数设置文本的位置，后面会详细介绍。::

    screen.draw.text("hello world", (20, 100))

``screen.draw.text`` 有很多可选的位置参数，下面详细介绍。

设置字体和字号
''''''''''''''''''

pgzero 从你运行的 python 文件所在目录的 ``fonts`` 目录加载字体，跟使用图片和声音的
方式类似。字体必须是 ``.ttf`` 格式。比如::

    screen.draw.text("hello world", (100, 100), fontname="Viga", fontsize=32)

关键字参数:

-  ``fontname``: 显示文本所用字体的名称。默认用系统字体。
-  ``fontsize``: 字体大小，默认值是 ``24``.
-  ``antialias``: 是否用抗锯齿模式显示文本，默认值 ``True``.

颜色和背景色
''''''''''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), color=(200, 200, 200), background="gray")

关键字参数:

-  ``color``: 文本的颜色，默认值是 ``white``.
-  ``background``: 文本的底色，默认是 ``None`` 也就是没有底色.

``color`` (以及 ``background``, ``ocolor``, ``scolor``, 和
``gcolor``) 可以是类似于 ``(255,127,0)`` 的 (r, g, b) 序列（元组）。也可以
用颜色的名字比如 ``"red"``，亦支持形如 ``"#FF7F00"` 的 HTML 格式的16进制颜色
字符串，或者形如 ``"0xFF7F00"`` 表示十六进制颜色的字符串。

如果 ``background`` 参数值设置为 ``None`` 那么，文字的底色就是透明的，但是用
 ``screen.draw.text`` 设置文字底色的效率统称不如用 ``pygame.font.Font.render`` 
 方法。所以如非必要，请勿设置文字的底色。

除了有轮廓和投影的不可见文本外，文字颜色不能指定透明度。 ``alpha`` 参数的介绍
中有更多关于透明色的介绍。

位置
'''''''''''

::

    screen.draw.text("hello world", centery=50, right=300)
    screen.draw.text("hello world", midtop=(400, 0))

关键字参数:

::

    top left bottom right
    topleft bottomleft topright bottomright
    midtop midleft midbottom midright
    center centerx centery

位置相关的关键字参数的用法与 ``pygame.Rect`` 类似，你可以用两个参数分别设置
文字（其实是文字的包围矩形）的位置，也可用用一个元组同时指定两个位置。
译者：这里提供的方法很多，还是需要多练习下，才能够灵活的组合使用不同的关键字
参数指定文字的位置。

如果用了多种方法重复设置了文字的位置，多余的关键字参数一定会被忽律，但是没有
很明显的优先级。为了限定文本的位置，``screen.draw.textbox`` 小结中将会详细
介绍。

单词换行
'''''''''

::

    screen.draw.text("splitting\nlines", (100, 100))
    screen.draw.text("splitting lines", (100, 100), width=60)

关键字参数:

-  ``width``: 以像素为单位设置文本绘制的最大宽度，默认是 ``None``，没限制
-  ``widthem``: 以基于字体的 em 为单位指定文本的最大绘制宽度，默认是 ``None``.
-  ``lineheight``: 行距, 基于字体默认行高.默认值是 ``1.0``，单倍行距。

``screen.draw.text`` 遇到换行符 (``\n``) 会换行，即便是设置了 ``width``
或者 ``widthem`` ，为了确保每行长度小于指定的文本长度也会城市换行。文本长度可能会
超过 ``width``或者 ``widthem`` ，因为换行总是在有空格的地方，如果一个一个
单词很长，不能适应以后的长度，这个长单词不会换行。轮廓和投影文本也是如此。
所以，文本的长度可能会超过给定的宽度。

当然你可以用特殊的 Unicode 字符 (``\u00A0``) 代替空格来组织默认的换行行为。

文本对齐
''''''''''''''

::

    screen.draw.text("hello\nworld", bottomright=(500, 400), align="left")

关键字参数:

-  ``align``: 水平对齐，默认为 ``None``.

``align`` 参数的作用，设置绘制多行文本时行与行之间的对齐方式。``align`` 参数
的有效值是字符串，``"left"``, ``"center"``, 或者 ``"right"``，0 到 1 之间的
小数或者是 ``None`` 。


如果 ``align`` 参数的值是 ``None`` ，文本对齐方式由其他参数决定，大部分时候可以
满足你的需求。诸如 (``topleft``, ``centerx``, etc.), ``anchor`` 等参数都可能会
影响对齐方式。建议使用默认对齐方式，如果对齐方式不符合你的预期，就用  ``align`` 
参数明确的指定对齐方式。

文字外轮廓
'''''''

::

    screen.draw.text("hello world", (100, 100), owidth=1, ocolor="blue")

外轮廓参数:

-  ``owidth``: 轮廓宽度，以轮廓宽度为标准，默认是 ``None``.
-  ``ocolor``: 轮廓颜色，默认是 ``"black"``.

如果指定了 ``owidth`` 参数的值，文本将会有外轮廓。如果 ``owidth`` 参数的
值过大，效果看起来可能不会太好。``owidth`` 的单位是经过精心选择的，``1.0``
是一个典型的比较合适的轮廓宽度只。需要特别注意的是，轮廓宽度的值是字体大小
除以 24 。

如果 ``color`` 设置为透明，比如 (e.g.``(0,0,0,0)``)，同时设定了 ``ocolor``
那么文字就是只有轮廓的镂空的文字，这个特性跟 ``gcolor`` 不兼容。 ``gcolor``
的有效值跟 ``color`` 的有效值相同。

投影
'''''''''''

::

    screen.draw.text("hello world", (100, 100), shadow=(1.0,1.0), scolor="blue")

关键字参数:

-  ``shadow``: (x,y) 投影水平和竖直方向的便宜. 默认是 ``None`` 没有投影.
-  ``scolor``: 投影的颜色，默认 ``"black"``.

如果指定了 ``shadow`` 参数的值，那么文本会有一个投影，值必须是只有2个元素的
序列类型（一般是元组），用来指定投影在水平和竖直方向的偏移，值可以是整数，负数。
比如 ``shadow=(1.0,1.0)`` 对应的文本右下角的投影， ``shadow=(0,-1.2)`` 
是文本上方的投影。

投影的单位也是一个经验值，投影的单位是字体大小除以 18。跟轮廓类似，假设字体
大小是 72，那么投影的单位就是 72/18=4 。投影和轮廓都是一个相对的值，不是
像素值。

如果 ``color`` 指定的是透明色，指定了 ``shadow``，那么文本就是中空带投影的
文本。

投影参数  ``scolor`` 的有效值跟轮廓颜色值和 ``color`` 颜色有效值一样。

渐变色
''''''''''''''

::

    screen.draw.text("hello world", (100, 100), color="black", gcolor="green")

关键字参数:

-  ``gcolor``: 渐变色底端的值，默认是 ``None``.

gpzero 中文本的渐变色是从上到下的线性渐变，顶部是 ``color`` 参数指定的颜色，
底部是 ``gcolor`` 指定的颜色。渐变色的起始和结束点以及渐变方向是写死的（硬
编码），无法通过参数设置。

依赖  ``pygame.surfarray`` ，模块，这个模块用到了 numpy 或者 其他数值库。

颜色透明度
''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), alpha=0.5)

关键字参数:

-  ``alpha``: 0 到 1 之间的透明度值. 默认为 ``1.0``.

In order to maximize reuse of cached transparent surfaces, the value of
``alpha`` is rounded.
为最大限度重用缓存的透明度面，``alpha`` 参数进行了四色五入。

依赖  ``pygame.surfarray`` ，模块，这个模块用到了 numpy 或者 其他数值库。

锚点定位
''''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), anchor=(0.3,0.7))

关键字参数:

-  ``anchor``: 指定了宽度和高度锚点系数的元组，长度为二，默认 ``(0.0, 0.0)``.

``anchor`` 指定了文本相对于指定位置的锚点，如果没有指定位置相关的关键字
参数，``anchor`` 参数的两个值可以是 ``0.0`` 到 ``1.0`` 之间的任意值。
默认锚点值 ``(0,0)`` ，锚点位于文本的左上角。 ``(1,1)`` 意味着锚点在
文本的右下角。

译注：如果用 flash 的使用经历，在 flash 的元件中，需要设置中心点，这个
中心点其实就是锚点。在 flash 中，元件的位置是中心点的位置，旋转、定位
都是通过这个中心点定位的。

旋转
''''''''

::

    screen.draw.text("hello world", (100, 100), angle=10)

关键字参数:

-  ``angle``: counterclockwise rotation angle in degrees. Defaults to
   ``0``.

旋转面的定位很棘手。旋转文本时，锚点的位置固定不变，文本围绕锚点旋转。假设一个文本
左上角的坐标是 ``(100, 100)``，旋转 ``90`` °，那么 文本所在的 Surface 绘制在左下角
坐标为 ``(100, 100)`` 的位置。

如果觉得不好理解，就把锚点定位在图形的中心。如果把锚点设置在中心，文本中心保持
不这么，无论你怎么旋转。

In order to maximize reuse of cached rotated surfaces, the value of
``angle`` is rounded to the nearest multiple of 3 degrees.
为了最大程度的利用缓存的旋转的面，``angle`` 的值会取最近的3的倍数的值。

译注：旋转操作很浪费资源，所以会缓存旋转的面，重复利用，类似于递归中用
表查询法，提高递归的效率。假设，你不停的旋转文字，那么只需要计算 ``120``
次旋转的结果，之后每次旋转过来后，查询并复用之前的面 ``surface`` 就行了
无需重新计算，所以效率会很高。这里属于优化的技巧，pgzero 库的作者有心。

受限制的文本
''''''''''''''''

::

    screen.draw.textbox("hello world", (100, 100, 200, 50))

``screen.draw.textbox`` requires two arguments: the text to be drawn, and a
``pygame.Rect`` or a ``Rect``-like object to stay within. The font size
will be chosen to be as large as possible while staying within the box.
Other than ``fontsize`` and positional arguments, you can pass all the
same keyword arguments to ``screen.draw.textbox`` as to ``screen.draw.text``.

``screen.draw.textbox`` 需要2个参数：需要绘制的文本以及一个 ``pygame.Rect`` 
或者 ``Rect` 对象。在盒子限定的范围内，文字会尽可能的大。除了指定位置和字体大小
的参数不能用，``screen.draw.textbox`` 方法的参数跟 ``screen.draw.text`` 参数
的用法相同。

译注：这里有点自适应的意思。就是画个框，往里面写文字，类似于 ``ppt`` 中的文本框
文字越多，字体越小。其实这个方法也可以指定一个最小的字体大小，如果计算字号小于
指定的最小字号，那么文字应该超出这个框。