Pygame Zero教程
===========================

.. highlight:: python
    :linenothreshold: 5

创建窗口
-----------------

首先，新建一个python空白文件，并保存为  ``intro.py``。

运行一下命令来验证文件可以正常运行并创建一个窗口 ::

    pgzrun intro.py


Pygame Zero所有设置都是默认可选，所以一个空的文件也是一个合法可以运行的Pygame Zero脚本！
好神奇有木有！


单击窗口的关闭按钮或者按 ``Ctrl+Q``(``⌘-Q`` on Mac)快捷键退出游戏。如果游戏卡住了，
你可以在终端窗口按 ```Ctrl+C``快捷键。

绘制窗口的背景
--------------------


然后，让我们添加一个 :func:`draw` 函数并且设置窗口的大小。每当需要刷新(重绘)窗口的时候，
Pygame Zero就会调用这个函数。

在 ``intro.py``,文件添加以下代码::

    WIDTH = 300
    HEIGHT = 300

    def draw():
        screen.fill((128, 0, 0))

重新运行 ``pgzrun intro.py``脚本，游戏窗口变成了红色的正方形。

这段代码的作用是啥？

``WIDTH`` 以及 ``HEIGHT`` 指明了窗口的宽和高. 
这段代码把窗口设置为300x300大小。

``screen`` 是内置的代表窗口显示的类. screen类有
:ref:`很多负责绘制精灵和图形的函数<screen>`. 
调用``screen.fill()`` 方法可以用指定一个颜色元组
``(red, green, blue)`` ，然后用纯色填充窗口. ``(128, 0, 0)`` 是暗红色. 试着
改变rgb颜色值，然后查看代码运行效果。

接下来让我们设置一个我们生成动画的精灵：


绘制一个精灵
-------------


在我们绘制任意东东之前，我们需要一个外星人alien精灵图片。你可以右键单击这张图，然后选择
图片另存为。

.. image:: _static/alien.png


这个精灵图片是支持透明色的png图片，非常适合在游戏里使用。这张图片为深色背景设计，所以
只有当你程序运行的时候才能够看到外星人的头盔。

.. tip::

    你可以在 `kenney.nl
    <https://kenney.nl/assets?q=2d>`_网站找到包括这张图在内的大量免费精灵图片. 这张
    图是 `Platformer Art Deluxe pack
    <https://kenney.nl/assets/platformer-art-deluxe>`_的一部分.


只有将图片保存在正确的路径Pygame Zero才能够加载图片。新建一个``images``目录，并且把
图片另存为``alien.png``。文件夹和文件名都是小写，虽然windows不区分文件名的大小写，但是
linux和OSX是区分，不然就会陷入一个跨平台兼容性的陷阱。


新建完图片并且另存图片之后，你的项目如下：

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    └── intro.py


``images/`` 目录是Pygame Zero查找代码中图片的标准默认位置。


内置类:class:`Actor`用来代表一个你绘制到屏幕的图形。

让我们来定义一个在屏幕上显示的图形，修改 ``intro.py`` 文件加载图片::

    alien = Actor('alien')
    alien.pos = 100, 56

    WIDTH = 500
    HEIGHT = alien.height + 20

    def draw():
        screen.clear()
        alien.draw()


哇塞，外星人显示在屏幕上了。通过把字符串 ``'alien'``作为参数传递给``Actor``类，Pygame
Zero自动加载了外星人精灵，并且图片具有位置和大小属性。这样我们就可以根据外星人alien的告诉
设置窗口的高度属性``HEIGHT``。

 ``alien.draw()``方法把外星人精灵绘制到屏幕上的当前位置。

移动外星人
----------------

我们先让外星人在舞台的外面; 修改``alien.pos`` 一行代码如下::

    alien.topright = 0, 10

Note how you can assign to ``topright`` to move the alien actor by its
top-right corner. 注意修改``topright``属性来相对于右上角来修改外星人角色位置
的方法。如果外星人角色的右边横坐标为``0``, 外星人角色恰好在屏幕的左侧. 然后，我们
让外星人角色动起来。在文件底部添加以下代码::

    def update():
        alien.left += 2
        if alien.left > WIDTH:
            alien.right = 0

Pygame Zero在每一帧都会调用 :func:`update`函数。通过在每一帧让外星人移动很小的像素数，
外星人就会在屏幕上从左向右滑过。一旦外星人左侧的坐标大于窗口的宽度，就让外星人回到左侧
重新向右滑动。

处理鼠标单击事件
---------------
接下来，我们让游戏在单击鼠标的时候，做点不一样的东西。为了实现这个目标我们需要定义
一个:func:`on_mouse_down`函数。在文件下方添加以下代码::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            print("Eek!")
        else:
            print("You missed me!")


运行游戏，并尝试多次单击外星人角色。

Pygame Zero可以非常聪明的处理你对于函数的调用。如果你定义的函数没有``pos``参数，Pygame
在调用函数的时候就不会传递位置参数。``on_mouse_down``方法还有一个``button``按钮参数，
代表单击的鼠标的那个键。因此我们也可以这样定义``on_mouse_down``函数::

    def on_mouse_down():
        print("You clicked!")

或者::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Eek!")


声音和图像
-----------------


接下来我们让外星人表现受伤的造型，保存一下文件:

* `alien_hurt.png <_static/alien_hurt.png>`_ -保存图片 ``alien_hurt.png``
  到 ``images`` 目录.
* `eep.wav <_static/eep.wav>`_ - 新建一个叫做 ``sounds``目录，然后保存``eep.wav``到声音目录。

这时候项目如下图所示:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    │   └── alien_hurt.png
    ├── sounds/
    │   └── eep.wav
    └── intro.py

``sounds/``是Pygame Zero查找声音文件的默认标准目录。
现在让我们用新的图片和声音资源改写 ``on_mouse_down`` 函数::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            sounds.eep.play()
            alien.image = 'alien_hurt'

当你单击外星人的时候，你会听到一段声音，精灵也会切换到不开心的外星人。

但是这个游戏还有一个bug，那就是被单击后外星人不会回到开心的造型，但是每次单击的
时候，声音会播放。接下来让我们改掉这个bug。


时钟函数
-----
如果出了游戏编程之外你对python非常熟悉，你就会知道用``time.sleep()`` 来插入延时。
你可以像下面这样写代码::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            sounds.eep.play()
            alien.image = 'alien_hurt'
            time.sleep(1)
            alien.image = 'alien'

但是不行的是，在游戏中这样写是不合适的。 ``time.sleep()``阻塞了所有的活动。我们希望
游戏能够继续运行和播放动画。实际上我们需要从``on_mouse_down``返回，然后让游戏在切换
外星人的造型之后还能够继续运行，让``draw()`` 和 ``update()``继续跑。

这可难不倒Pygame Zero，因为我们有一个内置的 :class:`Clock`，可以让函数延时执行。

首先，让我们重构也就是从新写代码。我们定一个设置外星人手上和返回普通造型的函数::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

运行代码跟之前没啥区别``set_alien_normal()``并没有被调用。但是我们可以用时钟类
修改 ``set_alien_hurt()`` 方法，这样``set_alien_normal()``就可以延迟一段时间被调用了::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 1.0)

``clock.schedule_unique()``可以让``set_alien_normal()`` 方法在
 ``1.0`` 秒后被调用. ``schedule_unique()`` 同时防止同一
函数在快速单击的时候被多次安排调用.

尝试下，你会发现外星人alien在1s后恢复正常形态。尝试快速单击外星人，验证外星人只有在最后
单击的1s之后才会恢复。


总结
-------

我们已经学习如何绘制精灵，播放声音，处理输入时间，以及使用内置
的时钟类。

也许你继续完善游戏，可以记录游戏的得分，或者让外星人alien移动的更加诡异。

有许多特性让Pygame Zero易于使用。访问 :doc:`内置对象<builtins>`学习如何使用其他API