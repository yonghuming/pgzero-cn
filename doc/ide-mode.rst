在IDLE和其他编辑器运行Pygame Zero
==========================================

我们通常用如下命令运行Pygame Zero::

    pgzrun my_program.py

在特定的程序,比如像IDLE这样的集成环境,以及Edublokcs中,会运行 ``python`` 而不是 ``pgzrun`` 命令.

但是呢,Pygame Zero已经很贴心的包含了编写可以通过 ``python`` 命令运行python程序的
方式, 为了能够用 ``python`` 运行程序,你只需要输入 ::

    import pgzrun

作为程序的第一行, 然后输入 ::

    pgzrun.go()

当做程序的最后一行.


例子
-------

下面是用Pygame Zero画圆的程序. 你可以把代码复制到IDLE运行.


    import pgzrun


    WIDTH = 800
    HEIGHT = 600

    def draw():
        screen.clear()
        screen.draw.circle((400, 300), 30, 'white')


    pgzrun.go()

