[TOC]

## 多场景切换流程

### 第一个场景启动顺序

```flow
init=>operation: HelloWorld::init函数
onEnter=>operation: HelloWorld::onEnter函数
onEnterDf=>operation: HelloWorld::onEnterTransitonDidFinish函数
init->onEnter->onEnterDf
```

### 多场景切换生命周期

####使用pushScene函数进入下一场景

```flow
secondinit=>operation: SecondScene::init函数
firsttranDidStart=>operation: FirstScene::onEnterTransitionDidStart函数
secondonEnter=>operation: SecondScene::onEnter 函数
firstonExit=>operation: FirstScene::onExit 函数
secondtranDidFinish=>operation: SecondScene::onEnterTransitionDidFinish 函数

secondinit->firsttranDidStart->secondonEnter->firstonExit->secondtranDidFinish
```

#### 使用replaceScene函数跳转SecondScene

```flow
secondinit=>operation: SecondScene::init函数
firsttranDidStart=>operation: FirstScene::onEnterTransitionDidStart函数
secondonEnter=>operation: SecondScene::onEnter 函数
firstonExit=>operation: FirstScene::onExit 函数
secondtranDidFinish=>operation: SecondScene::onEnterTransitionDidFinish 函数
firstCleanup=>operation: FirstScene::cleanup函数

secondinit->firsttranDidStart->secondonEnter->firstonExit->secondtranDidFinish->firstCleanup
```

#### 使用PopScene函数从返回FirstScene

```flow
secondtranDidStart=>operation: SecondScene::onEnterTransitionDidStart函数
secondonExit=>operation: SecondScene::onExit 函数
secondCleanup=>operation: SecondScene::cleanup函数
firstonEnter=>operation: FirstScene::onEnter 函数
firsttranDidFinish=>operation: FirstScene::onEnterTransitionDidFinish 函数

secondtranDidStart->secondonExit->secondCleanup->firstonEnter->firsttranDidFinish
```

