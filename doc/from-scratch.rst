从Scratch到Pygame Zero
=======================

这份教程将会比较用Scratch和Pygame Zero编写的Flappy Bird游戏。Scratch和Pygame Zero
的相似度是非常高的

提示: `Pygame Zero版本的Flappy Bird`__ 包含在了Pygame Zero的代码库中.

.. __: https://github.com/lordmauve/pgzero/blob/master/examples/flappybird/flappybird.py

你也可以从代码库中下载`Scratch` 版本 `Scratch version`__ 的Flappy Bird游戏.

.. __: https://github.com/lordmauve/pgzero/raw/master/examples/flappybird/Flappy%20Bird.sb

Pygame Zero版本的游戏包含了积分逻辑，但是为了更清晰对比二者实现，所以在这份教程里忽略了积分。

下面的Python脚本为讲解的更清晰重写了。


舞台
---------

在Scratch舞台上的内容弄是这样的：

.. image:: _static/scratch/flappybird-stage.png

除了舞台背景之外，还有三个对象，小鸟，上方管道和下方管道

对应的pygame Zero代码则是把这三个角色都设置为 ``Actors`` (表演者)::

   bird = Actor('bird1', (75, 200))
   pipe_top = Actor('top', anchor=('left', 'bottom'))
   pipe_bottom = Actor('bottom', anchor=('left', 'top'))

在Pygame Zero中我们必须确保我们绘制了这3个对象，原则上这使得绘制场景有更大的可伸缩性
和自由度::

   def draw():
       screen.blit('background', (0, 0))
       pipe_top.draw()
       pipe_bottom.draw()
       bird.draw()


管道移动
-------------

管道的移动速度是恒定不变的，与小鸟没有关系。当管道会不停的从屏幕的左边移动到右边，并且
在数值方向的位置是随机变化的。

在Scratch中我们可以通过给顶部和底部的管道添加两个脚本来实现管道的移动。

.. image:: _static/scratch/flappybird-top-start.png

.. image:: _static/scratch/flappybird-bottom-start.png

我们来概括的讲下这些脚本的作用：

* 当管道在屏幕左边即将出现的时候,条件 `x position < -240` 成立,并且会导致管道位置重置
* 变量 `pipe_height` 用来改变管道在数值方向的位置.上下管道之间的间隔应该是不变的,所以
  不能让两个管道的位置都是随机的,所以我们只在一个脚本设置了让pipe_height随机的逻辑.
* 脚本 ``set y position to pipe height +/- 230`` 让顶部的管道在 ``pipe_height`` 上方
  底部的管道在 ``pipe_height`` 下方.

在Pygame Zero中代码就简单多了.我们可以定义一个函数不停的更新两个管道.当然我把更新管道
的过程分成了两部分,这样管道重置的过程就更加清晰了::

   import random

   WIDTH = 400
   HEIGHT = 708
   GAP = 130
   SPEED = 3

   def reset_pipes():
       pipe_gap_y = random.randint(200, HEIGHT - 200)
       pipe_top.pos = (WIDTH, pipe_gap_y - GAP // 2)
       pipe_bottom.pos = (WIDTH, pipe_gap_y + GAP // 2)

   def update_pipes():
       pipe_top.left -= SPEED
       pipe_bottom.left -= SPEED
       if pipe_top.right < 0:
           reset_pipes()

与Scratch不同的是,我可以把需要重用的值定义为"常量", 用大写字幕表示. 当我需要调整游戏
的时候,我只需要修改一次值就好了.比如在上面的例子中,当我需要加大间隔或者减小间隔的时候
我只需要简单的修改 ``GAP`` 的值就好了.

与Scratch代码最大的不同在于, Python代码中没有 ``forever`` 循环. 这是Scratch和大多数
文本编程语言的区别: 你必须通过每次更新一小步动画的方式来更新游戏并且返回. 返回让pygame
Zero有机会处理处理输入或者重新绘制屏幕.不断的循环就会形成游戏了,所以每一个循环必须很快
完成.

当你需要更新游戏一点点内容的时候,Pygame Zero会调用 ``update()`` 函数,然后我们需要调
用 ``update_walls`` ::

   def update():
      update_walls()


小鸟
--------

上面介绍的如何把Scratch逻辑转换为Python代码的模式同样适用于小脚的逻辑.让我们先来看
一下小鸟的代码.

更新小鸟相关的代码被整合到了 ``update_bird`` 方法. 首先这段代码包含了让小鸟飞翔的逻辑::

   GRAVITY = 0.3

   # 初始化小鸟的状态
   bird.dead = False
   bird.vy = 0

   def update_bird():
       uy = bird.vy
       bird.vy += GRAVITY
       bird.y += bird.vy
       bird.x = 75

这是一个很简单的重力公式:

* GRAVITY 指的的是 **重力加速度** 常数
* acceleration 用来改变 **速度**
* 速度用来改变小鸟的 **位置**

我们需要建立一个 ``bird.vy`` 变量, 表示小鸟在 ``y`` 方向的速度.这是我们新定义的一个
变量,不是Pygame Zero提供给我们的

* 重力是向下的加速度常量: ``GRAVITY`` 大于0
* 加速度用来改变速度: 把 ``GRAVITY`` 加到 ``bird.vy`` 也就是小鸟竖直方向的速速
* 速度概念位置: 把 ``bird.vy`` 加到 ``bird.y`` 上 

注意!小鸟水平方向的位置是始终不变的,整个游戏中小鸟的 ``x`` 坐标始终是 75. 我们通过
让管道移动来模拟小鸟的运动.看起来就像是我们我们又一个跟着小鸟移动的镜头.因此我们在
游戏中并不需要 ``vx`` 变量.

The next section makes the bird flap its wings::
然后我们让小鸟可以拍打自己的翅膀::

       if not bird.dead:
           if bird.vy < -3:
               bird.image = 'bird2'
           else:
               bird.image = 'bird1'

检查小鸟是否向上或者向下移动. 如果小鸟快速的向上移动我们展示 ``bird2`` 图像,反之展示
``bird1`` 图像. -3是通过不断试错得出的经验值,这样游戏看起来会更加真实.

接下来我们要检测小鸟有没有撞墙::

       if bird.colliderect(pipe_top) or bird.colliderect(pipe_bottom):
           bird.dead = True
           bird.image = 'birddead'

如果撞墙了把变量 ``bird.dead`` 值设置为 ``True`` .这是一个 **布尔值** 表示非假即真.
我们可以方便的用布尔变量来检测小鸟是否还活着,如果小鸟挂了,我们就不需要处理玩家的输入了. 

最后我们检测一下小鸟有没有触地或者触顶, 如果是的话重置小鸟的状态和属性::

       if not 0 < bird.y < 720:
           bird.y = 200
           bird.dead = False
           bird.vy = 0
           reset_pipes()

重置小鸟的时候为什么要调用 ``reset_pipes()`` 呢? 是因为我把我管道的代码定义成一个
单独的函数,我可以在我需要重置游戏中的墙的时候调用这个函数.这样小鸟回到初始位置的是
玩家有反应的时间,这样才是一个好的游戏.

Again, this needs to be called every frame, so we add it to ``update()``::
再次重申,以上过程每一帧都要调用,因此我们把这些操作放到 ``update_date()`` 函数::

   def update():
      update_walls()
      update_bird()

最后我们在游戏逻辑里必须响应玩家控制.当我们单击键盘的时候,小鸟向上飞. 当你按了键盘
上的某一个键,Pygame Zero会调用 ``on_key_down`` 函数::

   FLAP_VELOCITY = -6.5

   def on_key_down():
       if not bird.dead:
           bird.vy = FLAP_VELOCITY

在这里,如果小鸟没挂的话,我们设置小鸟的 ``vy`` 纵向速度为一个复制:在Pygame Zero里
这意味着小鸟向上飞.

无论是Scratch还是Python代码,你都能够找到许多并行的代码:

.. image:: _static/scratch/flappybird-bird-start.png
.. image:: _static/scratch/flappybird-bird-space.png


Scratch和Pygame Zero最大的不同在于:

* 在Pygame Zero中你不能一直循环 - 只更新每一帧并且返回
* 坐标系不同.Pygame Zero,屏幕的左上角是原点 ``x = 0, y = 0``, ``x`` 跟Scratch一样是
  从左向右的,但是 ``y`` 轴指向屏幕下方! 这也是 ``GRAVITY`` 是正数而 ``FLAP_VELOCITY`` 
  是负数 了.
* ``bird.dead`` 是一个布尔值, 所以我们写 ``if not bird.dead`` 而不是像Scratch一样
  写 ``dead = 0``.

总结
-------

Many of the concepts available in Scratch can be translated directly into
Pygame Zero.
许多Scratch中的概念可以直接迁移到Pygame Zero中.

下面是一些对比:

+----------------------------+--------------------------------------------+
| In Scratch                 | In Pygame Zero                             |
+============================+============================================+
| ``change y by 1`` (up)     | ``bird.y -= 1``                            |
+----------------------------+--------------------------------------------+
| ``change y by -1`` (down)  | ``bird.y += 1``                            |
+----------------------------+--------------------------------------------+
| ``set costume to <name>``  | ``bird.image = 'name'``                    |
+----------------------------+--------------------------------------------+
| ``if dead = 0``            | ``if not bird.dead:``                      |
+----------------------------+--------------------------------------------+
| ``set dead to 0``          | ``bird.dead = False``                      |
+----------------------------+--------------------------------------------+
| ``if touching Top?``       | ``if bird.colliderect(pipe_top)``          |
+----------------------------+--------------------------------------------+
| ``When Flag clicked``...   | Put code into the ``update()`` function.   |
| ``forever``                |                                            |
+----------------------------+--------------------------------------------+
| ``When [any] key pressed`` | ``def on_key_down():``                     |
+----------------------------+--------------------------------------------+
| ``pick random a to b``     | ``import random`` to load the ``random``   |
|                            | module, then ``random.randint(a, b)``      |
+----------------------------+--------------------------------------------+
| (0, 0) 是屏幕中心          | (0, 0) 是屏幕的左上角                      |
+----------------------------+--------------------------------------------+

在有些情况下, Python中的代码更简单,因为Python的代码可以写成更易于阅读的方式.

Pygame Zero中的角色使得我们更加方便的使用坐标系.我们使用 ``anchor`` 中心点位置来决定
管道的位置, 我们通过检测管道的 ``pipe_top.right < 0`` 而不是 ``if x position < -240`` 
来判断关掉是否离开屏幕.
