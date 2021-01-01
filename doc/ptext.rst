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

Specify ``gcolor`` to color the text with a vertical color gradient. The
text's color will be ``color`` at the top and ``gcolor`` at the bottom.
Positioning of the gradient stops and orientation of the gradient are
hard coded and cannot be specified.

Requries ``pygame.surfarray`` module, which uses numpy or Numeric
library.

Alpha transparency
''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), alpha=0.5)

Keyword argument:

-  ``alpha``: alpha transparency value, between 0 and 1. Defaults to
   ``1.0``.

In order to maximize reuse of cached transparent surfaces, the value of
``alpha`` is rounded.

Requires ``pygame.surfarray`` module, which uses numpy or Numeric
library.

Anchored positioning
''''''''''''''''''''

::

    screen.draw.text("hello world", (100, 100), anchor=(0.3,0.7))

Keyword argument:

-  ``anchor``: a length-2 sequence of horizontal and vertical anchor
   fractions. Defaults to ``(0.0, 0.0)``.

``anchor`` specifies how the text is anchored to the given position,
when no positioning keyword arguments are passed. The two values in
``anchor`` can take arbitrary values between ``0.0`` and ``1.0``. An
``anchor`` value of ``(0,0)``, the default, means that the given
position is the top left of the text. A value of ``(1,1)`` means the
given position is the bottom right of the text.

Rotation
''''''''

::

    screen.draw.text("hello world", (100, 100), angle=10)

Keyword argument:

-  ``angle``: counterclockwise rotation angle in degrees. Defaults to
   ``0``.

Positioning of rotated surfaces is tricky. When drawing rotated text, the
anchor point, the position you actually specify, remains fixed, and the text
rotates around it. For instance, if you specify the top left of the text to be
at ``(100, 100)`` with an angle of ``90``, then the Surface will actually be
drawn so that its bottom left is at ``(100, 100)``.

If you find that confusing, try specifying the center. If you anchor the
text at the center, then the center will remain fixed, no matter how you
rotate it.

In order to maximize reuse of cached rotated surfaces, the value of
``angle`` is rounded to the nearest multiple of 3 degrees.


Constrained text
''''''''''''''''

::

    screen.draw.textbox("hello world", (100, 100, 200, 50))

``screen.draw.textbox`` requires two arguments: the text to be drawn, and a
``pygame.Rect`` or a ``Rect``-like object to stay within. The font size
will be chosen to be as large as possible while staying within the box.
Other than ``fontsize`` and positional arguments, you can pass all the
same keyword arguments to ``screen.draw.textbox`` as to ``screen.draw.text``.
