---
layout: post
title: "记录一次ToLua内存泄漏的检查"
date: 15:03:54
category: Unity
---

在Unity里引入了ToLua之后，有些游戏资源一直驻留在内存里。最近就开始找了些工具（如LuaProfiler，UWA）来跟踪lua table的生命周期，结果是没有发现异常。
然后还是不断删减代码，反复比较运行的结果，来查找内存没有释放的原因。

简单的模型像下面这样，一个游戏对象里绑定了两个组件，其中LuaBehaviour引用了lua环境里的table。

    GameObject
       |-- SpriteRenderer
       |-- LuaBehaviour
             |-- LuaTable

    LuaTable
       |-- self.spriteRenderer

同时地LuaTable里又持有这个游戏对象下面的SpriteRenderer组件的引用。下图显示的是LuaTable在引用C#对象时会为这一对象生成一个lua_userdata的数据结构。

![图片pic1]({{ "/images/20190510/lua.jpg" | absolute_url }})

在反复的测试之后，结论是回收资源需要执行下面的过程。

    LuaState.LuaGC()
    System.GC.Collect ()
    Resources.UnloadUnusedAssets()

### 回收过程

当场景切换的时候，LuaBehaviour会触发OnDestroy方法，这里我调用了LuaTable的Dispose。

    void OnDestroy () {
        ...

        mLuaTable.Dispose ();

        ...
    }

在Dispose之后，lua_userdata结构并没有被即时回收，就是说lua_userdata和SpriteRenderer的引用仍然存在，而SpriteRenderer持有的资源也不会被释放。

因此这里第一步需要执行LuaGC来回收lua_userdata的结构，第二步调用C#的GC回收SpriteRenderer组件，第三步调用UnloadUnusedAssets就能够回收占用的图片资源了。

收工！