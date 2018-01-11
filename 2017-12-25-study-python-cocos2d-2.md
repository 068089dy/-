---
cocos2d 骨骼动画
---

## 简介
cocos2d-python自带一个简易的动画制作工具animator.py，一个简单的骨架编辑工具skeleton_editor.py，和一个简单的骨架查看器view_skeleton.py；
需要下载cocos2d的源码包(https://pypi.python.org/pypi/cocos2d/0.6.5)
```
#解压源码包
$ tar -zxvf cocos2d-0.6.5.tar.gz 
#打开文件夹
$ cd cocos2d-0.6.5
#显示文件
$ ls
AUTHORS.txt       docgen            LICENSE.python_interpreter  setup.cfg
benchmarks        docs              MANIFEST.in                 setup.py
CHANGELOG         INSTALL           NEWS.txt                    test
cocos             LICENSE           PKG-INFO                    tools
cocos2d.egg-info  LICENSE.euclid    README.rst                  utest
CONTRIBUTING.rst  LICENSE.grossini  samples
```
view_skeleton.py在tools文件夹下
animator.py在tools/skeleton文件夹下
skeleton_editor.py也在tools/skeleton文件夹下

## 查看骨架
在test文件夹下有个sample_skeleton.py骨架示例文件，我们用view_skeleton.py查看一哈：
```
$ python tools/view_skeleton.py test/sample_skeleton.py 
```
## 编辑骨架
首先，写一个简单的骨架和皮肤文件：
```
#root_bone.py：骨架文件
from cocos.skeleton import Bone,Skeleton
#param:骨骼名字，长度，顺时针旋转角度，（位置(x)，位置(y)）
root_bone = Bone('body',70,180,(0.0,0.0))
skeleton = Skeleton(root_bone)
```

```
#root_skin.py：皮肤文件
#param:名字，相对骨骼偏移，皮肤，
skin = (['body',(25,91),'gil-cuerpo.png',True,True,0.5],)
```

```
$ python tools/skeleton/skeleton_editor.py root_bone.py root_skin.py 
```

## 骨骼动画制作
```
$ python tools/skeleton/animator.py test/sample_skeleton.py --skin test/sample_skin.py test/SAMPLE.anim 
```
__操作方法：__  
左右键：一帧一帧移动  
‘s’：保存  
空格：播放动画  
‘+’：在当前位置添加一个关键帧  
’-‘：删除当前位置的关键帧  
‘page up’：跳到前一个关键帧  
‘page down’：跳到下一个关键帧  
‘home’：跳到第一个关键帧  
‘end’：跳到最后一个关键帧  
‘insert’：在当前位置之前添加一个单位时间（一般是1/16秒）段  
‘delete’：删除当前位置之后的一个单位时间段  

github上的简易教程（已停止维护）
https://github.com/liamrahav/cocos2d-python-tutorials

---
cocos2d 事件
---

## 键盘
```
import cocos
import pyglet
class KeyDisplay(cocos.layer.Layer):
    
    # If you want that your layer receives director.window events
    # you must set this variable to 'True'
    is_event_handler = True

    def __init__(self):

        super( KeyDisplay, self ).__init__()

        self.text = cocos.text.Label("", x=100, y=280 )

        # To keep track of which keys are pressed:
        self.keys_pressed = set()
        self.update_text()
        self.add(self.text)

    def update_text(self):
        key_names = [pyglet.window.key.symbol_string (k) for k in self.keys_pressed]
        text = 'Keys: '+','.join (key_names)
        # Update self.text
        self.text.element.text = text
    
    def on_key_press (self, key, modifiers):
        """This function is called when a key is pressed.
        'key' is a constant indicating which key was pressed.
        'modifiers' is a bitwise or of several constants indicating which
            modifiers are active at the time of the press (ctrl, shift, capslock, etc.)
        """

        self.keys_pressed.add (key)
        self.update_text()

    def on_key_release (self, key, modifiers):
        """This function is called when a key is released.

        'key' is a constant indicating which key was pressed.
        'modifiers' is a bitwise or of several constants indicating which
            modifiers are active at the time of the press (ctrl, shift, capslock, etc.)

        Constants are the ones from pyglet.window.key
        """

        self.keys_pressed.remove (key)
        self.update_text()

    def update_text(self):
        key_names = [pyglet.window.key.symbol_string (k) for k in self.keys_pressed]
        text = 'Keys: '+','.join (key_names)
        # Update self.text
        self.text.element.text = text

director.init()
main_sence = cocos.scene.Scene(KeyDisplay())
director.run(main_sence)
```

---
cocosnode
---