Class

# SKNode

大多数SpriteKit类的基类。

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
- [声明](#declaration)
- [概述](#overview)
- [节点的特性](#overview01)
- [主题](#topics)
- [继承关系](#Relationships)

<a name="declaration"></a>
## 声明
### iOS, tvOS

```Objective-c
@interface SKNode : UIResponder
```

### macOS

```Objective-c
@interface SKNode : NSResponder
```
### watchOS

```Objective-c
@interface SKNode : NSObject
```

<a name="overview"></a>
## 概述

SKNode类不会绘制任何可视的内容，它主要的角色是给其他节点类提供可使用的基本功能。所有基于SpriteKit游戏
的可视元素都是使用预设的SKNode的子类绘制的。

<a name="overview01"></a>
## 节点的特性
节点被分层的组织为节点树，类似于视图和子视图的工作方式。最常见的是，场景节点（[SKScene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKScene/SKScene.md)）被定义为节点树的根节点，其他内容作为子节点。场景节点提供一个动画循环，处理节点上的动作、物理模拟，然后渲染节点树的内容显示出来。

节点树中的每一个节点都为其子节点提供一个坐标系统。将子节点添加到节点树之后，通过设置其[position]()属性，将其定位到父节点的坐标系之中。可以通过修改节点的[xScale]()、[yScale]()、[zRotation]()属性来缩放和旋转的坐标系统。当一个节点的坐标系统被缩放或旋转时，这些修改会同时应用到节点本身和它的子节点上。

虽然节点本身不会绘制任何内容，但是许多节点的子类会绘制可视化的内容，因此SKNode类包含一些可视化的概念：

- frame属性为节点的可视化内容提供了边框，这些内容由旋转和缩放属性来修改。如果节点的类绘制内容那么frame内是非空的。每个节点子类以不同的方式来定义内容的大小。在一些子类中，节点内容的尺寸是显示声明的，例如在SKSpriteNode类中。在其他子类中，内容的大小是由使用类使用其他的对象属性来隐式计算的。例如SKLabelNode对象对用文本的文字和字体特性来决定其内容的大小。

- 可以通过调用节点的[calculateAccumulatedFrame]()方法来计算包含该节点和他子节点所有的frame累积的最大矩形范围。

- 其他属性，例如[alpha]()和[hidden]()属性，影响节点及其子节点如何绘制。

所有的节点都是响应器对象，它们可以直接响应用户与屏幕上节点的交互。你可以在坐标系之间进行转换并测试 hit testing来确定节点响应的点。你也可以在节点树中执行交集，来确定它们的物理区域是否有重叠。

节点树中的任何节点都可以运行actions（动作），这些动作用来定义在节点的动画属性、添加或移除节点、播放声音或者执行其他任何。动作是SpriteKit动画系统的核心。

节点可以支持物理实体，物理实体是用来模拟物理属性的对象。当一个节点拥有物理实体时，物理模拟会自动计算物理物体的新位置，然后移动和旋转节点来匹配这个位置。

节点提供了约束来表示与场景中其他节点或位置的关系。这些约束在场景被渲染之前被场景自动应用。

对节点的操作必须在主线程中进行。SKViewDelegate、SKSceneDelegate和SKScene的所有SpriteKit回调都发生在主线程中，这些都是对节点进行操作的安全位置。

