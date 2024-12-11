
# 华为鸿蒙开发：深入探索Tabs组件的定制与应用


## 引言


在移动应用设计中，标签页（Tabs）是用户切换不同内容区块的重要界面元素。华为鸿蒙操作系统提供的`Tabs`组件支持开发者创建高度定制化的标签页界面。本文将通过 DevEco Studio 详细介绍`Tabs`组件的使用，包括基本设置、动态生成标签页、以及如何通过自定义组件来实现独特的视觉效果。


## Tabs组件基础


`Tabs`组件允许开发者创建一组可滑动的标签页，每个标签页对应不同的内容区域。


### 基本使用


#### 示例1：创建基础Tabs标签页



```
@Entry
@Component
struct Index {
  build() {
    Tabs() {
      TabContent() {
        Text('探索内容') // 有且只能一个子组件
      }
      .tabBar('探索') // 配置导航

      TabContent() {
        Text('趋势内容') // 有且只能一个子组件
      }
      .tabBar('趋势')

      TabContent() {
        Text('热点内容') // 有且只能一个子组件
      }
      .tabBar('热点')

      TabContent() {
        Text('个人中心内容') // 有且只能一个子组件
      }
      .tabBar('个人中心')
    }
  }
}


```

在这个示例中，我们创建了一个包含四个标签页的`Tabs`组件，每个标签页显示不同主题的文本内容。


### 配置导航


#### 示例2：配置Tabs导航属性



```
@Entry
@Component
struct Index {
  build() {
    Tabs({
      barPosition: BarPosition.Start // 导航位置
    })
    {
      TabContent() {
        Text('探索内容')
      }
      .tabBar('探索')

      TabContent() {
        Text('趋势内容')
      }
      .tabBar('趋势')

      TabContent() {
        Text('热点内容')
      }
      .tabBar('热点')

      TabContent() {
        Text('个人中心内容')
      }
      .tabBar('个人中心')
    }
    .vertical(false) // 调整导航水平或垂直
    .scrollable(false) // 是否开启手势滑动
    .animationDuration(0) // 点击滑动的动画时间
  }
}


```

在这个示例中，我们配置了`Tabs`组件的导航属性，包括导航位置、滚动手势和动画时间。


### 动态生成Tabs


#### 示例3：使用ForEach动态生成Tabs



```
@Entry
@Component
struct Index {
  titles: string[] = [
    '探索', '趋势', '热点', '科技', '娱乐',
    '体育', '教育', '健康', '财经', '旅游'
  ]

  build() {
    Tabs() {
      ForEach(this.titles, (item: string, index) =&gt; {
        TabContent() {
          Text(`${item}内容`)
        }
        .tabBar(item)
      })
    }
    .barMode(BarMode.Scrollable) // 实现滚动导航栏
  }
}


```

在这个示例中，我们使用`ForEach`循环动态生成了10个标签页，每个标签页的标题都是从`titles`数组中获取的。


### 自定义Tabs样式


#### 示例4：自定义Tabs样式



```
@Entry
@Component
struct Index {
  @Builder
  myBuilder(title: string, img: ResourceStr) {
    Column() {
      Image(img)
        .width(30)
      Text(title)
    }
  }

  build() {
    Tabs({
      barPosition: BarPosition.End
    })
    {
      TabContent() {
        Text('消息中心内容')
      }
      .tabBar(this.myBuilder('消息中心', $r('app.media.ic_tabbar_icon_1')))

      TabContent() {
        Text('设置中心内容')
      }
      .tabBar(this.myBuilder('设置中心', $r('app.media.ic_tabbar_icon_2')))
    }
  }
}


```

在这个示例中，我们定义了一个`@Builder`函数`myBuilder`来自定义每个标签页的样式，包括图片和文本。


### 状态管理


#### 示例5：管理Tabs激活状态



```
@Entry
@Component
struct Index {
  @State selectedIndex: number = 0

  @Builder
  myBuilder(itemIndex: number, title: string, img: ResourceStr, selImg: ResourceStr) {
    Column() {
      Image(itemIndex == this.selectedIndex ? selImg : img)
        .width(30)
      Text(title)
        .fontColor(itemIndex == this.selectedIndex ? Color.Red : Color.Black)
    }
  }

  build() {
    Tabs()
    {
      TabContent() {
        Text('消息中心内容')
      }
      .tabBar(this.myBuilder(0, '消息中心', $r('app.media.ic_tabbar_icon_1'), $r('app.media.ic_tabbar_icon_1_selected')))

      TabContent() {
        Text('设置中心内容')
      }
      .tabBar(this.myBuilder(1, '设置中心', $r('app.media.ic_tabbar_icon_2'), $r('app.media.ic_tabbar_icon_2_selected')))
    }
    .onChange((index: number) =&gt; {
      this.selectedIndex = index
    })
    .animationDuration(0)
    .scrollable(false)
  }
}


```

在这个示例中，我们使用`@State`属性`selectedIndex`来存储当前激活的标签页索引，并在`onChange`事件中更新这个索引。


### 特殊形状的Tab


#### 示例6：添加特殊形状的Tab



```
@Entry
@Component
struct Index {
  @State selectedIndex: number = 0

  @Builder
  myBuilder(itemIndex: number, title: string, img: ResourceStr, selImg: ResourceStr) {
    Column() {
      Image(itemIndex == this.selectedIndex ? selImg : img)
        .width(30)
      Text(title)
        .fontColor(itemIndex == this.selectedIndex ? Color.Red : Color.Black)
    }
  }

  @Builder
  centerBuilder() {
    Image($r('app.media.ic_special_icon'))
      .width(40)
      .margin({ bottom: 10 })
  }

  build() {
    Tabs()
    {
      TabContent() {
        Text('首页内容')
      }
      .tabBar(this.myBuilder(0, '首页', $r('app.media.ic_tabbar_icon_0'), $r('app.media.ic_tabbar_icon_0_selected')))

      TabContent() {
        Text('分类内容')
      }
      .tabBar(this.myBuilder(1, '分类', $r('app.media.ic_tabbar_icon_1'), $r('app.media.ic_tabbar_icon_1_selected')))

      TabContent() {
        Text('活动内容')
      }
      .tabBar(this.centerBuilder())

      TabContent() {
        Text('购物车内容')
      }
      .tabBar(this.myBuilder(3, '购物车', $r('app.media.ic_tabbar_icon_2'), $r('app.media.ic_tabbar_icon_2_selected')))

      TabContent() {
        Text('个人中心内容')
      }
      .tabBar(this.myBuilder(4, '个人中心', $r('app.media.ic_tabbar_icon_3'), $r('app.media.ic_tabbar_icon_3_selected')))
    }
    .onChange((index: number) =&gt; {
      this.selectedIndex = index
    })
    .animationDuration(0)
    .scrollable(false)
  }
}


```

在这个示例中，我们添加了一个特殊形状的标签页，并使用`centerBuilder`函数来自定义其样式。


## 结语


通过本文的详细介绍和示例，你应该能够掌握`Tabs`组件的基本使用、自定义样式以及状态管理。这些技能对于开发具有丰富标签页功能的鸿蒙应用至关重要。如果你有任何问题或想要进一步讨论，欢迎在评论区留下你的想法。




---


以上就是一篇关于华为鸿蒙开发中`Tabs`组件的详细教程。希望这篇文章能帮助你更好地理解和使用华为鸿蒙开发中的`Tabs`组件。如果你在使用 DevEco Studio 进行开发时遇到任何问题，欢迎交流讨论。


 本博客参考[slower加速器](https://jisuanqi.org)。转载请注明出处！
