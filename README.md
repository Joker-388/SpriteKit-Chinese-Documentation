SpriteKit中文文档
======================================

# 前言
因为找不到系统全面的中文学习资料，并且官方文档也存在问题，比如示例代码缺失等诸多问题，所以自己对官方文档进行的翻译，并参考swift实例代码重写了Objective-C的实例代码，并通过搜集旧版本的实例代码，补全代码逻辑上的缺失，尽可能提供完整的代码片段。因为作者也是刚刚了解这个框架，并且英文能力有限，有不正确的地方希望包涵和指正。

项目起始于2018.11.28，开始肯定有很多空链接，作者也会不停的完善，但因为时间有限，希望更多对SpriteKit框架感兴趣的中文开发者参与进来一同完成这个大工程吧。

# SpriteKit
使用经过优化的动画系统、物理模拟和事件处理框架，来创建基于sprite（精灵）的2D游戏吧。

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
- [精灵的内容通过场景在视图中显示](#overview01)
- [场景中显示的内容由节点树来决定](#overview02)
- [纹理提供可重用的图形数据](#overview03)
- [动画由节点执行动作来实现](#overview04)
- [在场景中添加物理实体和关节来模拟物理世界](#overview05)
- [SpriteKit入门](#overview06)
- [创建你的第一个场景](#overview07)
- [主题](#topics)

<a name="overview"></a>
## 概述

SpriteKit是一个使纹理图像产生动画的图形渲染和动画功能基础框架，也称之为sprites（精灵）。SpriteKit提供了一个传统的渲染循环来更新将展示的内容并渲染。你来决定展示内容的结构和他们如何动态的变化。SpriteKit利用图形硬件有效的渲染。SpriteKit针对动画和内容的改变进行了优化，这种设计更适合需要灵活处理动画的游戏和应用程序。

<a name="overview01"></a>
### 精灵的内容通过场景在视图中显示

动画和渲染由[SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)的对象执行。将这个视图添加到窗口中，然后由它来绘制内容。由于它是一个视图，所以它可以与其他视图通过视图的层级结构进行组合。
将你的游戏内容组织到场景中，场景由[SKScene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKScene/SKScene.md)对象表示。一个场景包含精灵（Sprites）和其他需要被渲染的内容。一个场景实现了每一帧刷新的内容处理步骤和逻辑。任何时候，当视图展示场景后，就会自动执行动画和每一帧的逻辑。
要使用SpriteKit创建一个游戏或应用，你可以创建[SKScene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKScene/SKScene.md)的子类，还可以实现场景的的代理方法来处理主要的游戏相关的任务。例如，你可以创建单独的场景类来分别显示主菜单、游戏场景和游戏结束显示的内容。你可以使用[SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)的对象在窗口中实现不同场景的切换。当你切换场景时，你可以使用[SKTransition]()类来实现场景切换的动画。

<a name="overview02"></a>
### 场景中显示的内容由节点树来决定

[SKScene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKScene/SKScene.md)类是[SKNode]()类的子类的子类（SKScene:SKEffectNode:SKNode）。当使用SpriteKit时，节点用来将所有内容组织起来，场景对象作为节点树中其他节点的根节点，场景对象决定绘制内容和如何渲染。

每一个节点的位置在其父节点定义的坐标系中指定，节点还将其他属性应用于其内容及其后代的内容。例如，当旋转节点时，它的所有后代也会旋转。你可以使用节点树构建复杂图像，然后通过调整最顶层节点的属性来旋转，缩放和混合整个图像。

[SKNode]()类不会绘制任何东西，但是它的特性传递给了它的子类。每种可绘制内容都由SpriteKit中的不同子类表示。其他一些节点子类不会绘制自己的内容，而是修改其后代的行为。例如，你可以在场景中 使用一个[SKEffectNode]()对象对整个子树应用一个 Core Image 滤镜。通过控制节点树的结构可以控制节点的展示顺序。

所有的节点对象都是响应器对象，继承自UIRespinder或NSRespinder，因为创建任何节点对象的子类来接收用户的输入操作。视图类自动扩展响应者链以包括场景的节点树。

请参考[SKNode]()

<a name="overview03"></a>
### 纹理提供可重用的图形数据

纹理(Textures)，抽象为[SKTexture]()对象，是用于渲染精灵的共享图像。当需要将相同的图像应用于多个精灵时，你应当使用纹理。通常你通过app的bundle中加载图片数据来创建纹理。然后SpriteKit也可以利用运行时的其他来源，如Core Graphics图像甚至节点树来创建纹理。

Sprite Kit 通过处理较低级别的代码需求来加载纹理和并让它们对图形硬件可用，来简化了纹理的管理。然而如果你的游戏使用大量图像，则可以通过控制部分过程来提高其性能。首先，通过明确告诉SpriteKit加载纹理来做到这一点。

纹理图册是在你的游戏中一起使用的一组相关的纹理。例如，你可以使用一个纹理图册存储让 一个角色活动需要的所有纹理或渲染游戏级别的背景需要的所有瓷砖。Sprite Kit 用纹理图册来提高渲染性能。

请参考[SKTexture]()和[SKTextureAtlas]()

<a name="overview04"></a>
### 动画由节点执行动作来实现

场景内容执行做作(actions)来实现动画。每一个动作都是一个[SKAction]()类的对象。你来通知节点来执行动作,然后当场景处理动画帧时，执行动作。一些动作在一帧动画就完成，而另一些在完成前应用于多帧动画。动作最常见的用途是用来改变节点的属性并由动画展示。例如，你可以创建动作来对一个节点进行移动、缩放、旋转或更改透明度。另外，动作也可以更改节点树、播放声音甚至执行自定义的代码。

动作是非常有用的，但你也可以组合动作来创建更复杂的效果。你可以创建一组同时运行或顺序 运行的动作。你可以让动作自动重复。
场景中也能执行自定义的每帧处理。覆盖你的场景子类的方法来执行额外的游戏任务。例如，如 果一个节点需要每帧移动，你可能会直接每帧地调整其属性而不是使用一个动作来这样做。

请参考[SKAction]()

<a name="overview05"></a>
### 在场景中添加物理实体和关节来模拟物理世界

虽然你可以控制场景中每一个节点的确切位置，但你通常会希望这些节点能够相互交互，相互碰撞并产生速度的变化。你可能还希望执行不是由动作控制的运动，比如模拟重力或者其他力。为此，你可以创建一个物理实体，一个[SKPhysicsBody]()类的对象，并将它添加给场景中的节点。每个物理主体由形状，大小，质量和其他物理特征来定义。

通过内场景添加一个[SKPhysicsWorld]()类的对象，可以为场景定义全局特性的物理模型。你可以为场景的物理世界定义重力和力的速度来作为场景的全局模拟属性。当为场景添加物理实体后，场景会模拟物理作用于这些实体。一些力，如摩擦力和重力会自动生效。向场景中添加[SKFieldNode]()对象可以其他力自动应用于关联的物理实体。

你也可以直接修改其速度或直接向其施加力或脉冲来直接影响特定的实体。计算每个物体的加速度和速度，并使物体相互碰撞。然后，在模拟完成之后，更新相应节点的位置和旋转。

您可以精确控制哪些物理效果相互作用。例如，您可以指定特定物理场节点仅影响场景中物理实体的子集。您还可以决定哪些物理实体可以相互碰撞，并分别决定哪些交互会导致您的应用被调用。您可以使用这些回调来添加游戏逻辑。例如，当物理体被另一个物理体撞击时，你的游戏可能会销毁一个节点。

你也可以在场景中利用物理世界查找物理实体，并且使用关节(joint)来使物理实体之间产生联系。根据关节的类型将实体模拟在一起。

请参考[Simulating Physics]()

<a name="overview06"></a>
## SpriteKit入门

SpriteKit将内容实现为分层的节点树结构。节点树由场景作为根节点和其他提供内容的节点构成。处理场景的每个帧并将其渲染到视图。场景执行动作和物理事件模拟，这两者都改变了树的内容。然后使用SpriteKit高效地渲染场景。
要开始学习SpriteKit，您应该按照以下顺序查看这些类，然后再转到框架中的其他类：
- [SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)
- [SKScene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKScene/SKScene.md)
- [SKNode]()
- [SKSpriteNode]()
- [SKAction]()
- [SKPhysicsBody]()

<a name="overview07"></a>
#### 创建你的第一个场景

SpriteKit像其他可视控件一样将内容展示在窗口中，SpriteKit的内容通过SKView类渲染。SKView渲染的内容为场景，它是一个SKScene对象。场景参与响应链并使其具有适合游戏的其他功能。

由于SpriteKit内容由视图对象呈现，因此你可以将此视图与视图层次结构中的其他视图组合在一起。例如，你可以使用一个按钮控件（UIButton）并将它放在SpriteKit视图的上面，或者你可以添加一个有交互功能的精灵来实现自己定义的按钮，具体怎样做由你自己决定。稍后的例子中，你将会看到如何实现在场景中实现用户的交互。

一个SKView可以添加到一个UIView的子视图中，或者你可以通过自定义控制器或着通过代码替换你的控制器的view为SKView的对象视图。下面将展示如何通过重写控制器的viewDidLoad 方法来实现替换控制器的视图为SKView：

代码 1:将控制器的视图替换为SKView
```
@property (nonatomic, strong, readonly) SKView *skView;

- (void)viewDidLoad {
    [super viewDidLoad];
    self.view = [[SKView alloc] initWithFrame:[UIScreen mainScreen].bounds];
}

- (SKView *)skView {
    return (SKView *)self.view;
}
```

创建SpriteKit视图后，下一步就是创建一个场景来显示内容。通常你需要创建一个SKScene类的子类来满足你不同的需求，但这里为了简洁，创建了一个SKScene对象并展示它：

代码 2:创建并展示一个场景
```
@property (nonatomic, strong) SKScene *scene;

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    [self.skView presentScene:self.scene];
}

- (SKScene *)scene {
    if (!_scene) {
        _scene = [[SKScene alloc] initWithSize:self.skView.frame.size];
    }
    return _scene;
}
```

要在SpriteKit中显示内容，还需要在场景中添加相关的节点作为场景的子节点。最后的实例代码就是创建一个SKLabelNode（Label节点）添加为场景的子节点来显示它：

代码 3:在场景中添加一个label
```
@property (nonatomic, strong) SKLabelNode *label;

self.label.position = CGPointMake(CGRectGetMidX(self.scene.frame), CGRectGetMidY(self.scene.frame));
[self.scene addChild:self.label];

- (SKLabelNode *)label {
    if (!_label) {
        _label = [SKLabelNode labelNodeWithText:@"SpriteKit"];
    }
    return _label;
}
```

<a name="topics"></a>
## 主题
### 在你的应用中显示SpriteKit内容

创建SpriteKit用于表示和展示内容的基本对象

[SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)

显示SKSpriteKit内容的对象，显示的内容有SKScene对象提供。

[SKScene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKScene/SKScene.md)

视图中所有SpriteKit显示对象的根节点。

[SKNode]()

大多数SpriteKit类的基本类（所有的节点类都从该类派生。它不绘制任何东西）。

[SKViewDelegate]()

允许SKView对象动态控制渲染速率。

[SKSceneDelegate]()

你的应用可以实现参与SpriteKit动画循环的方法。

### 绘制内容的节点

生成可显示形状，纹理，图像和视频的可视节点。

[SKSpriteNode]()

绘制矩形纹理，图像或颜色的节点。

[SKShapeNode]()

渲染一个由Core Graphics路径定义的形状的节点。

[SKLabelNode]()

显示文本标签的节点。

[SKVideoNode]()

播放视频内容的节点。

[SKCropNode]()

该节点用于剪裁其子节点的部分区域，来使其子节点只有部分区域绘制到帧缓冲区。

[SKReferenceNode]()

A node that creates its children from an archived collection of other nodes.

### 使用纹理

使用这些类从可以在SpriteKit游戏或应用程序中使用的图像资源生成纹理。



