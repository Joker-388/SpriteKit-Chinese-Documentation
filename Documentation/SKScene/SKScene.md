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
- [主题](#topics)
- [继承关系](#Relationships)

<a name="overview"></a>
## 概述

一个SKScene对象代表SpriteKit的一个场景。场景是SpriteKit节点([SKNode]())数中的根节点。这些节点提供场景动画和显示的内容。你需要用一个[SKView](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/SKView.md)对象来显示场景。

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
```
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

## 场景如何处理动画框架



