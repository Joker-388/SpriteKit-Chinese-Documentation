Class

# SKScene
所有SpriteKit显示内容的根视图

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
- [创建场景](#createscene)
- [场景如何处理动画帧](#animation)
- [使用场景的代理](#delegate)
- [场景中的后处理](#processing)
- [子类说明](#subclass)
- [主题](#topics)
- [继承关系](#Relationships)
- [另请参阅](#seealose)

<a name="overview"></a>
## 概述

一个SKScene对象代表SpriteKit的一个场景。场景是SpriteKit节点([SKNode]())数中的根节点。这些节点提供场景动画和显示的内容。你需要用一个[SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)对象来显示场景。
<a name="createscene"></a>
### 创建场景

一个景场景由视图来显示，场景包括定义场景原点位置和场景大小的属性。如果场景和视图大小不一致，可以定义场景缩放方式来适应视图。

#### 场景的尺寸定义了他的可见区域

当场景第一次初始化时，需要指定size。场景大小以点为单位来指定场景的可见部分的大小。但是仅用于指定场景的可见部分。节点数可以在该区域之外，但节点依然会被场景处理，只是被渲染器忽略。

#### 使用锚点在视图中定位场景的坐标系统

默认情况下，场景的原点位于视图的左下角，因此，初始化高度为1024、宽度为768的场景，原点(0,0)位于左下角，坐标(1024,768)位于右上角。frame属性保持(0,0)-(1024,768)。

场景的position属性被Scene Kit忽略，因为场景总是节点树的根节点。它的默认值是0并且不能更改它。但是，您可以通过设置场景的锚点属性来移动场景的原点。

场景锚点的默认值是0，它位于左下角。场景的可见坐标空间是(0,0)到(width,height)。对于不滚动场景内容的游戏，默认的锚点是最有用的。

第二个最常见的锚点值是(0.5,0.5)，它将场景的原点置于视图的中间，场景的可见坐标空间为(-width/2，-height/2)到(width/2,height/2)。当你想要很容易地定位相对于屏幕中心的节点时，例如在滚动游戏中，以场景的锚点为中心是非常有用的。然而，使用[SKCameraNode]()可以更好地实现这种效果。

因此，总的来说，anchorPoint和size属性用来设置场景的frame，它包含了场景的可见部分。

#### 场景的内容会被缩放以适应视图

场景被渲染后，其内容被复制到呈现视图中。如果视图和场景大小相同，那么内容可以直接复制到视图中。如果两者不同，那么场景会被缩放以适应视图。scaleMode属性决定内容如何被缩放。

当你设计你的游戏时，你应该决定一个处理场景大小和缩放属性的策略。以下是最常见的策略:

- 定义一个固定的场景尺寸并且永远不要改变它，选择一个缩放模式，让视图缩放场景的内容。给场景一个可预期的坐标系和布局结构。然后将美术资源和游戏逻辑建立在这个坐标系上。
- 调整游戏场景的大小，必要的时候，调整游戏的逻辑的美术资源以匹配场景的大小。
- 将scaleMode属性设置为SKSceneScaleModeResizeFill。SpriteKit会自动调整场景的大小，使其始终与视图的大小匹配。必要的时候，调整游戏的逻辑的美术资源以匹配场景的大小。

下面展示了使用固定大小场景的实现，这段代码指定了场景第一次显示的方法。它配置了场景的属性，包括缩放模式，然后添加内容。在这个例子中，缩放模式被设置为SKSceneScaleModeAspectFit，它在两个维度上对内容进行同等的缩放，并确保场景的所有内容都是可见的。
``` Objective-c
// 控制中执行的代码
HelloScene *hello = [[HelloScene alloc] initWithSize:CGSizeMake(768,1024)];
SKView *spriteView =(SKView *)self.view;
[spriteView presentScene:hello];

// 场景的定义
@interface HelloScene()

@property BOOL contentCreated;

@end

- (self)didMoveToView:(SKView *)view {
  if(!self.contentCreated) {
    [self createSceneContents];
    self.contentCreated = YES;
  }
}

- (void)createSceneContents {
  self.scaleMode = SKSceneScaleModeAspectFit;
  self.backgroundColor = [SKColor blueColor];
  ...
}
```

如果你希望场景大小在运行时发生变化，那么应该使用初始场景大小来确定要使用哪些美术资产，以及依赖于场景大小的任何游戏逻辑。你的游戏还应该重写场景的didChangeSize:方法，该方法在场景改变大小时被调用。当调用此方法时，应更新场景内容以匹配新的场景大小。

<a name="animation"></a>
### 场景如何处理动画帧
在传统视图系统中，视图内容渲染一次后，然后只有当模型(model)的内容发生变化时才会 再次渲染。这种模式对于视图非常适用，因为在实践中，大多数视图内容是静态的。另一方面， Sprite Kit 是明确为动态内容设计的。Sprite Kit 不断更新的场景内容并渲染它，以确保动画是 平滑和精确的。

动画和渲染场景的过程绑定到场景对象(SKScene)上。场景和动作处理只在场景被呈现时运 行。呈现的场景运行一个渲染循环，该循环在处理场景的节点树和渲染它之间交替进行。这种模 式类似于在大多数游戏中使用的渲染和处理循环。

每次通过渲染循环，场景的内容都会被更新然后被渲染。你不能重写渲染行为，而是要更新场景中的节点。但是，场景包含可以重写的方法来定制场景处理，并且你可以使用动作和物理来修改树中节点的属性。下面是渲染循环中的步骤:

- 1，场景循环从场景的[update:]()方法被调用开始，这是实现你自己游 戏内模拟(in-game simulation)的主要位置，包括输入处理、人工智能、游戏脚本和 其他类似的游戏逻辑。
- 2，场景处理树中的所有节点上的动作。它找到任何正在运行的操作，并将那些更改应用到树 上。在实践中，因为自定义操作，你还可以挂接到动作进程(mechanism)调用你自己 的代码。<br>你不能直接控制动作的处理的顺序，也不能让场景跳过某个节点上的动作，除非你从这些 节点中移除动作或从节点树中移除节点。
- 3，在帧的所有动作都已处理后，场景的didEvaluateActions方法被调用。
- 4，然后场景对场景中的物理体模拟物理。模拟物理的最终结果是，物理模拟可能会调节树中节点的位置和旋转角度。你的游戏也可以在物 理体之间互相接触时接收到回调。
- 5，场景的didSimulatePhysics方法在所有物理帧被模拟之后被调用。
- 6，场景应用与场景中关联的节点的任何约束。约束用于在场景中建立联系。例如，你可以应用一个约束，确保一个节点总是指向另一个节点而不管它是如何移动的。通过使用约束，你可以避免在场景处理中编写大量自定义代码。
- 7，场景调用它的didApplyConstraints方法。
- 8，场景调用它的didFinishUpdate方法。这是你改变场景的最后机会。
- 9，场景被渲染。

<a name="delegate"></a>
### 使用场景的代理
虽然使用场景的子类是处理场景的常用方法，但是你可以通过使用代理对象来避免它。上面描述的所有场景处理方法在SKSceneDelegate协议中都有相关的方法。如果向场景提供了一个代理，而该代理实现了这些方法中的一个，则调用该方法而不是场景中的那个方法。例如，iOS应用程序可能使用控制器作为代理。

<a name="processing"></a>
### 场景中的后处理
场景可以以任意顺序处理场景树中的动作。由于这个原因，如果你有一些需要在每一帧运行的任 务，且你需要在它们运行时精确地控制它们，你应该使用 didEvaluateActions 和 didSimulatePhysics 方法来执行这些任务。通常情况下，你在这里进行的更改，需要树中 的某些节点的最终计算出来的位置。你的后处理(post-processing)可以利用这些位置，并在树上执行其他有用的工作。

<a name="subclass"></a>
### 子类说明
通常，你的游戏会子类化一个场景来提供游戏玩法。你的子类通常:
- 创建场景的初始内容
- 实现每次处理帧时发生的游戏逻辑
- 实现响应器方法来处理键盘、鼠标或触摸事件

继承SKScene类的另一种模式是使用实现大部分游戏逻辑的代理方法。例如，在iOS游戏中，你可以让视图控制器成为场景的代理。视图控制器已经参与了事件处理，并且可以执行上面描述的所有其他必要职责。

<a name="topics"></a>
## 主题

### 初始化场景
[+ sceneWithSize:]()<br>

创建并返回一个新的场景对象。

[- initWithSize:]()<br>

初始化一个新的场景对象。

### 确定场景的哪个部分在视图中可见

[camera]()

场景中的摄像机节点，用于确定场景的哪一部分坐标空间在视图中可见。

[anchorPoint]()

视图中与场景原点对其的点，决定场景frame中(0, 0)坐标的位置。如果锚点为(0, 0)则场景原点即其frame中的(0, 0)点在场景坐标系的左下角，其右上角的坐标为(场景的宽度，场景的高度)。如果锚点为(0.5, 0.5)则其场景的原点即其frame中的(0, 0)点在场景的中心，即右上角的坐标为(场景的宽度 * 0.5，场景的高度 * 0.5)。

[size]()

场景的尺寸。

[- didChangeSize:]()

当场景的size改变时被调用。

[scaleMode]()

定义如何将场景映射到显示它的视图中，即场景的缩放模式。

### 设置场景的背景色

[backgroundColor]()

场景的背景色

### 视图和场景中的坐标系转换

[- convertPointFromView:]()

将一个点从视图坐标转换为场景坐标。

[- convertPointToView:]()

将点从场景坐标转换为视图坐标。

### 场景显示相关方法

[- sceneDidLoad]()

在场景被初始化或解码后立即调用。

[- willMoveFromView:]()

在场景从视图中移除之前立即调用。

[- didMoveToView:]()

在场景由视图显示后立即调用。

[view]()

当前显示场景的视图。

### 执行动画循环相关方法

[delegate]()

动画循环中会被调用的代理。

[- update:]()

在场景动作执行前对场景进行特定的更新。

[- didEvaluateActions]()

在场景动作执行后对场景进行特定的更新。

[- didSimulatePhysics]()

执行物理模拟之后需要进行的场景特定更新。

[- didApplyConstraints]()

执行应用约束后需要进行的特定于场景的更新。

[- didFinishUpdate]() 

在场景完成处理动画所需的所有步骤后调用，这是你改变场景的最后机会。

### 在场景中使用物理模拟

[physicsWorld]()

与场景相关的物理模拟。

### 在场景中使用音频

向场景中添加音频的最简单方法是添加一个SKAudioNode的子节点:

```Objective-c
- (void)didMoveToView:(SKView *)view {
    SKAudioNode *audioNode = [[SKAudioNode alloc] initWithFileNamed:@"music.mp3"];
    [self addChild:audioNode];
}
```

[audioEngine]()

播放场景中音频节点音频的AVFoundation音频引擎，可以用来进行播放相关操作，如声音大小、暂停播放等。

[listener]()

用于确定场景中音频监听位置的节点。

<a name="Relationships"></a>
## 继承关系

### 继承自

[SKEffectNode]()

<a name="seealose"></a>
## 另请参阅

### 在应用程序中显示SpriteKit内容

[SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)

显示SKSpriteKit内容的对象，显示的内容有SKScene对象提供。

[SKNode]()

大多数SpriteKit类的基本类（所有的节点类都从该类派生。它不绘制任何东西）。

[SKViewDelegate]()

允许SKView对象动态控制渲染速率。

[SKSceneDelegate]()

你的应用可以实现参与SpriteKit动画循环的方法。





