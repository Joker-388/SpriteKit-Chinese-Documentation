Class

# SKView
显示[SpriteKit](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/README.md)内容的对象，内容由SKSence对象提供。

语言
- Objective-C

SDKS
- iOS 7.0+
- macOS 10.9+
- tvOS 9.0+
- watchOS 3.0+

Framework
- SpriteKit

本页包含内容：
- [概述](#overview)
- [主题](#topics)
- [继承关系](#Relationships)

<a name="overview"></a>
## 概述

你可以通过调用视图的[presentScene:](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/02presentScene.md)方法来显示一个场景，当一个场景被视图显示时，它会在动画内容和渲染内容之间进行交替显示。可以设置SKView的paused属性为true来设置暂停。

<a name="topics"></a>
## 主题

### 显示场景

<a name="presentScene"></a>
[- presentScene:](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/02presentScene.md)

显示一个场景。

[- presentSecne:transition:](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/03presentScene:transition:.md)

从当前场景转场到一个新的场景。

[scene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/04scene.md)

当先显示的场景。

### 配置场景如何渲染

[delegate](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/05SKViewDelegate.md)

允许动态配置视图渲染速率的委托协议。

[preferredFrameRate]()

Deprecated

[preferredFramesPerSecond]()

视图渲染场景的动画帧速率。

[asynchronous]()

是否异步显示内容。

[frameInterval]()

在调用场景以更新其内容之前必须经过的帧数。Deprecated

[allowsTransparency]()

是否使用透明度渲染视图。

[ignoresSiblingOrder]()

父子节点和兄弟节点的关系是否影响场景中节点渲染的顺序。

[shouldCullNonVisibleNodes]()

视图是否自动从渲染数中移除不可见的节点。

### 暂停场景模拟

[paused]()

视图的场景动画是否暂停。

### 显示调试信息

[showsFPS]()

是否显示帧速率。

[showsQuadCount]()

是否显示渲染场景的矩形数。

[showsDrawCount]()

是否显示渲染视图所需的绘图数量。

[showsNodeCount]()

是否显示场景中可见的物理实体。

[showsPhysics]()

是否显示物理相关的调试信息。

[showsFields]()

是否显示物理场相关信息。

### 视图和场景之间坐标系转换

[- convertPoint:fromScene:]()

将一个点从场景坐标转换为视图图标。

[- convertPoint:toScene:]()

将一个点从视图坐标转换为场景坐标。

### 获取场景纹理

[- textureFromNode:crop:]()

渲染节点内容的一部分，并将显示的图像作为SpriteKit纹理返回。

[- textureFromNode:]()

渲染节点树的内容并将渲染的图像作为SpriteKit纹理返回。

<a name="Relationships"></a>
## 继承关系

### 继承自

[NSView](),[UIView]()

### 遵从协议

[NSSecureCoding]()
