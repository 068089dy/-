---
title:cocos2d动作
date:
---
原文（http://python.cocos2d.org/doc/programming_guide/actions.html）

import cocos
from cocos.actions import *
class hello(cocos.layer.ColorLayer):
#layer = cocos.layer.Layer()
    def __init__(self):
        super(hello,self).__init__(64,64,224,255)
        sprite = cocos.sprite.Sprite('icon.png')
        #设置精灵中心位置
        sprite.position = 320,240
        #放大三倍
        sprite.scale = 3
        self.add(sprite,z = 1)
        #repeat:重复执行动作；reverse：执行相反动作
        sprite.do(要执行的动作)

cocos.director.director.init()
main_scene = cocos.scene.Scene(hello())
cocos.director.director.run(main_scene)

## 动作：

### 1.基础动作
导入 from cocos.actions import *
#### position（位置相关）：
1.MoveBy（移动）
简单使用方法：
sprite.do(MoveBy( (50,100), duration=2 )) #2秒向右移动50px，向上移动100px
2.MoveTo
简单使用方法：
MoveTo((50,10), 8) #8s将精灵中心移动到（50，10）的坐标
3.JumpBy
简单使用方法：
JumpBy((100,100),200, 5, 6) #6s将精灵跳动5次，跳到向右移动100向上移动100的位置，每次跳动高度为200
4.JumpTo
简单使用方法：
JumpTo((100,100),200, 5, 6) #6s将精灵跳动5次，跳到坐标（100，100）的位置，每次跳动高度为200
5.Bezier
简单使用方法：
6.Place（立即移动）
使用方法：
sprite.do(Place( (120, 330) )) #中心立即移动到（120，330）位置
#### scale（大小比例相关）：
1.ScaleBy（放大）
使用方法：
sprite.do(ScaleBy(3,duration = 2)) #2秒放大三倍
2.ScaleTo（放大）
#### rotation（轮流，旋转）：
1.RotateBy
2.RotateTo
#### visible（可见）：
1.Show（显示）
2.Hide（隐藏）
3.Blink（眨）
4.ToggleVisibility（切换可见）
#### opacity（不透明度）：
1.FadeIn（淡入）
简单使用方法：
FadeIn(2) #两秒淡入
2.FadeOut（淡出）
简单使用方法：
FadeOut(2) #两秒淡出
3.FadeTo
简单使用方法：
FadeTo(128, 2) #两秒淡化到128（0为消失，255为显示）
### 2.特殊动作
#### 1.time related（时间相关）
1.Delay
简单使用方法：
Delay(5)+FadeTo(0, 2) #延时5秒后执行FadeTo(0, 2)
2.RandomDelay（随机延时）
#### 2.flow control（流控制，回调）
CallFunc
CallFuncS
#### 3.grid helpers（网格助手）
1.StopGrid
简单使用方法：
sprite.do(Waves3D( duration=2) + StopGrid()) #执行完Waves3D( duration=2)后恢复网格
2.ReuseGrid
简单使用方法：
没发现有什么作用
#### 4.camera related（相机相关）
OrbitCamera
简单使用方法：
没看懂
## 3.组成和修改动作
## 4.特效
## 5.创建自己的动作
### 1.基本类
### 2.即时动作
class SetOpacity( InstantAction ):
    def init(self, opacity):
        self.opacity = opacity
    def start(self):
        self.target.opacity = self.opacity
### 3.间隔动作
class FadeOut( IntervalAction ):
    def init( self, duration ):
        self.duration = duration

    def update( self, t ):
        self.target.opacity = 255 * (1-t)

    def __reversed__(self):
        return FadeIn( self.duration )
### 4.网格动作
这些是IntervalAction动作，而不是修改正常属性，如旋转，位置，缩放，它们修改网格属性。
让我们详细了解如何构建基本的非平铺网格动作：
class Shaky3D( Grid3DAction):
Shaky3D是Grid3DAction的子类，因此我们正在构建一个非平铺操作。如果我们要创建一个平铺操作，我们需要继承TiledGrid3DAction类：
def init( self, randrange=6, *args, **kw ):
    '''
    :Parameters:
        `randrange` : int
            Number that will be used in random.randrange( -randrange, randrange) to do the effect
    '''
    super(Shaky3D,self).init(*args,**kw)

    #: random range of the shaky effect
    self.randrange = randrange
我们的类接收randrange参数，所以我们保存它，我们调用我们的super类的init：
def update( self, t ):
    for i in xrange(0, self.grid.x+1):
        for j in xrange(0, self.grid.y+1):
            x,y,z = self.get_original_vertex(i,j)
            x += random.randrange( -self.randrange, self.randrange+1 )
            y += random.randrange( -self.randrange, self.randrange+1 )
            z += random.randrange( -self.randrange, self.randrange+1 )

            self.set_vertex( i,j, (x,y,z) )
像任何其他IntervalAction操作一样，每个帧都会调用一次update方法。所以，我们的Shaky3D效果会通过random.randrange函数计算的随机数来修改x，``y``和z坐标。
get_original_vertex方法返回顶点x和y的原始坐标，而get_vertex方法返回顶点x和y的当前坐标。            
