---
title: 虚拟Dom与Diff算法
categories: react
tag:
  - 前端
  - react
  - 基础认知
toc: true
thumbnail: "/images/redux.jpg"
---
在`react`框架中，采用虚拟`dom`,我们可以不用担心性能问题而随时随地的进行整个界面的刷新。由虚拟`DOM`来确认当界面真正发生变化的时候，只对需要变化的局部的`DOM`进行操作。
<!--more-->
### 什么是 DOM Diff 算法
当web的某一个部分发生变化的时候，就是对应的`DOM`节点发生了变化，由react来比较两个界面的区别，这就需要对于`DOM`树进行`Diff`算法的分析。对于标准的`Diff`算法的复杂度是`O(n^3)`，这样的复杂度是没有办法满足性能上的要求，如果每一次的界面都可以整体刷新界面的目的，我们一定要对`Diff`算法进行优化，他们基于`web`界面的特点做了两个简单的假设，使得`Diff`算法的复杂度直接的降低到了`O(n)`
- 两个相同组件产生类似的`DOM`结构，不同的组件产生`DOM`结构
- 对于同一层次的一组子节点，可以通过`id`进行区分。

算法上的优化是`React`整个界面的`Render`的基础，事实上也证明这两个假设是合理而且精确的，保证了整体界面构建的性能。

### 不同节点类型的比较
在`react`中比较两个虚拟`DOM`节点，当两个节点不同的时候，应该如何进行处理，主要分成两种情况：
(1)节点类型不同
(2)节点类型相同，但是属性不同

当在树的同一个位置前后输出不同类型的界面，`react`的具体操作是直接删除前面的节点，然后创建并插入新的节点，我们下面看一个具体的例子：
```
renderA: <div />
renderB: <span />
=> [removeNode <div />], [insertNode <span />]
```
简单来说就是先将`A`节点删除掉，然后插入`B`节点，在这个构成中我们需要注意的是：删除节点意味着彻底的销毁了这个节点，而不是在后续的比较中看是否有另外一个节点等于改删除的节点，如果被删除的节点有子节点的话，那么子节点也会被删除，不会在后面的继续比较，这也是复杂度降低的主要原因。
上面提到的是对虚拟` DOM `节点的操作，而同样的逻辑也被用在` React `组件的比较。
```
renderA: <Header />
renderB: <Content />
=> [removeNode <Header />], [insertNode <Content />]
```
`React` 在同一个位置遇到不同的组件时，也是简单的销毁第一个组件，而把新创建的组件加上去。这正是应用了第一个假设，不同的组件一般会产生不一样的 `DOM` 结构，与其浪费时间去比较它们基本上不会等价的 `DOM` 结构，还不如完全创建一个新的组件加上去,将大量的比较时间节省了时间，通过时间我们发现 `React `的 `DOM Diff `算法实际上只会对树进行逐层比较：
#### 逐层进行节点比较

而在 `React` 中，树的算法其实非常简单，那就是两棵树只会对同一层次的节点进行比较。如下图所示：
![逐层比较的截图](https://upload-images.jianshu.io/upload_images/13681871-a7efd556d3ae8548.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

React 只会对相同颜色方框内的 DOM 节点进行比较，即同一个父节点下的所有子节点。当发现节点已经不存在，则该节点及其子节点会被完全删除掉，不会用于进一步的比较。这样只需要对树进行一次遍历，便能完成整个 DOM 树的比较。

![详情](https://upload-images.jianshu.io/upload_images/13681871-056d58fe78eb2109.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面的过程就是现将`A`节点以及`A`的子节点都删除，然后将重新创造一个上面的结构，然后将这个结构整体挂到D的后面。
因为` React` 只会简单的考虑同层节点的位置变换，对于不同层的节点，只有简单的创建和删除。当根节点发现子节点中` A` 不见了，就会直接销毁` A`；而当` D `发现自己多了一个子节点` A`，则会创建一个新的` A `作为子节点。因此对于这种结构的转变的实际操作是：
```javaScript
A.destroy();
A = new A();
A.append(new B());
A.append(new C());
D.append(A);
```
#### 由 DOM Diff 算法理解组件的生命周期
我们再来看一下`React`的生命周期，其中的每一个阶段其实都和`DOM Diff`算法是息息相关的：
- `constructor`: 构造函数，组件被创建时执行；
- `componentDidMount`: 当组件添加到 `DOM` 树之后执行；
- `componentWillUnmount`: 当组件从` DOM` 树中移除之后执行，在 `React` 中可以认为组件被销毁；
- `componentDidUpdate`: 当组件更新时执行。
#### 相同类型节点的比较
第二种节点的比较是相同类型的节点，算法就相对简单而容易理解。`React` 会对属性进行重设从而实现节点的转换。例如:
```
renderA: <div id="before" />
renderB: <div id="after" />
=> [replaceAttribute id "after"]
```
虚拟` DOM `的` style `属性稍有不同，其值并不是一个简单字符串而必须为一个对象，因此转换过程如下：
```
renderA: <div style={{color: 'red'}} />
renderB: <div style={{fontWeight: 'bold'}} />
=> [removeStyle color], [addStyle font-weight 'bold']
```
#### 列表节点的比较
上面介绍了对于不在同一层的节点的比较，即使它们完全一样，也会销毁并重新创建。那么当它们在同一层时，又是如何处理的呢？`React` 在遇到列表时却又找不到`key` 时提示的警告。虽然无视这条警告大部分界面也会正确工作，但这通常意味着潜在的性能问题。因为 `React` 觉得自己可能无法高效的去更新这个列表。
我们可以从一个例子看一下这个问题：

![实现的操作](https://upload-images.jianshu.io/upload_images/13681871-8099fa41e1013705.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当我们没有为每一个节点设置唯一标识的时候，`React` 无法识别每一个节点，那么更新过程会很低效，即，将 `C` 更新成 `F`，`D `更新成 `C`，`E` 更新成` D`，最后再插入一个` E` 节点。效果如下图所示：

![操作过程](https://upload-images.jianshu.io/upload_images/13681871-55a39466ab8491f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`React `会逐个对节点进行更新，转换到目标节点。而最后插入新的节点` E`，涉及到的 `DOM` 操作非常多。而如果给每个节点唯一的标识（`key`），那么 `React `能够找到正确的位置去插入新的节点，入下图所示：

![操作的过程](https://upload-images.jianshu.io/upload_images/13681871-d2e1f4c73fbeda7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 结束语

本文分析了 `React` 的 `DOM Diff` 算法究竟是如何工作的，其复杂度控制在了` O（n）`，这让我们考虑 UI 时可以完全基于状态来每次` render `整个界面而无需担心性能问题，简化了 `UI` 开发的复杂度。而算法优化的基础是文章开头提到的两个假设，以及 `React `的` UI `基于组件这样的一个机制。理解虚拟 `DOM Diff` 算法不仅能够帮助我们理解组件的生命周期，而且也对我们实现自定义组件时如何进一步优化性能具有指导意义。
