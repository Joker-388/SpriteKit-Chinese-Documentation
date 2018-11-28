Protocol

# SKViewDelegate

允许动态配置视图渲染速率的委托协议。

## 概览

通过遵守SKViewDelegate的协议的对象设置SpriteKit视图的委托，你可以精确控制游戏或应用程序的帧速率。你可以选择这样做，以便为计算密集型代码或模拟电影等特效保持一致的帧速率。

[shouldRenderAtTime:]()的返回值不会改变SpriteKit场景中物理模拟和动作的速度。但是，如果返回NO, SpriteKit将跳过更新，并且不调用SKSceneDelegate方法。

## 主题

### Instance Methods

> [- view:shouldRenderAtTime:]()
>> 指定视图是否应在给定时间呈现。

## 继承关系

### 继承自
NSObject
