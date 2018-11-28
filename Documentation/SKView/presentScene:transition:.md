Instance Method

# presentScene:transition:

从当前场景转场到新场景。

## 声明

```
- (void)presentScene:(SKScene *)scene transition:(SKTransition *)transition;
```

## 参数

> scene
>> 要显示的场景

> transition
>> 用于两个场景之间过渡的动画

## 描述

如果当前视图正在展示一个场景，那么视图的[scene](https://github.com/Joker-388/SpriteKit-Chinese-Documentation/blob/master/Documentation/SKView/scene.md)属性会立即更新，转场会在两个场景切换之间执行。否则，新的场景会立即显示，转场参数会被忽略。

## 相关文档

[SKTransition]()
