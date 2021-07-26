### **react/react-dom调试**

1. 从`facebook/react`项目`master`分支拉取最新源码
2. 基于最新源码构建`react`、`scheduler`、`react-dom`三个包
3. 通过`create-react-app`创建测试项目，并使用步骤2创建的包作为项目依赖的包

###### **拉取源码**

拉取`facebook/react`代码

```sh
# 拉取代码
git clone https://github.com/facebook/react.git

# 如果拉取速度很慢，可以考虑如下2个方案：

# 1. 使用cnpm代理
git clone https://github.com.cnpmjs.org/facebook/react

# 2. 使用码云的镜像（一天会与react同步一次）
git clone https://gitee.com/mirrors/react.git
```

安装依赖

```sh
# 切入到react源码所在文件夹
cd react

# 安装依赖
yarn
```

打包`react`、`scheduler`、`react-dom`三个包为dev环境可以使用的`cjs`包。

```sh
# 执行打包命令
yarn build react/index,react/jsx,react-dom/index,scheduler --type=NODE

#此处可能会需要jdk，我已上传至鲸盘
http://pan.jd.com/sharedInfo/FD8D57B7F122397AEEC946AFBC5707A7
```

现在源码目录`build/node_modules`下会生成最新代码的包。我们为`react`、`react-dom`创建`yarn link`。

> 通过`yarn link`可以改变项目中依赖包的目录指向

```sh
cd build/node_modules/react
# 申明react指向
yarn link
cd build/node_modules/react-dom
# 申明react-dom指向
yarn link
```

###### 创建项目

接下来我们通过`create-react-app`在其他地方创建新项目。这里我们随意起名，比如“react-demo”。

```sh
npx create-react-app a-react-demo
```

在新项目中，将`react`与`react-dom`2个包指向`facebook/react`下我们刚才生成的包。

```sh
# 将项目内的react react-dom指向之前申明的包
yarn link react react-dom
```

现在试试在`react/build/node_modules/react-dom/cjs/react-dom.development.js`中随意打印些东西。

在`a-react-demo`项目下执行`yarn start`。现在浏览器控制台已经可以打印出我们输入的东西了。

通过以上方法，我们的运行时代码就和`React`最新代码一致了。



### **调用栈**



###### 1、运行项目

###### 2、打开浏览器开发工具

###### 3、打开Performace页签

###### 4、点击开始录制

###### 5、刷新页面

###### 6、点击停止

###### 7、查看调用栈

​	1.查找到入口index.js

​	2.查看到两个大的调用栈——fn和render

​	3.fn→_webpack_require→react-dom.development.js  这条调用栈其实就是我们写的import的部分

​	4.render(这个render是什么？没错，就是ReactDOM.render(),整个函数的入口就是这里，整个渲染过程，就是这一整块)，render函数做的事情大体可以分为三部分→左中右^_^

​		先看左半部分：一个框架的入口刚进入所进行的操作必然是初始化必要的数据，查看方法名称可知legacyCreateRootFromDOMContainer→createFiberRoot第一步创建整个应用的根fiber节点→createHostRootFiber→createFiber(为何又要创建fiber？ReactDOM.render可以调用多次，这些是由谁来管理的？FiberRoot)

​		继续往下看：listenToNativeEvent (初始化事件相关，暂时不看)

​		继续向下：unbatchedUpdates 出现了update，也就是更新  →它调用了scheduleUpdateOnFiber(在fiber上调度update，此时就是一个调度的过程)

​			react主要三个模块：schedule 调度	scheduler 

​													render  协调 	  reconciler	fiber dfs(diff) update

​													commit 渲染	 renderer  ReactDOM ReactNative

​	继续向下：renderRootSync这是一个分水岭，与它同级的是commitRoot，一个是render，一个是commit,react的三个阶段分别是schedule,render和commit,也就意味着每次更新react都会做这些事情

schedule是调度更新

render是决定本次更新改变哪些节点

commit则是将决定更新的元素更新，也就是去更新视图，所以，在commit 阶段之前，我们的视图是不会变化的，所有变化均在内存中

```javascript
// 为了验证，可在source页签下，找到我们通过yarn link 来链接的本地文件react-dom.development.js，在其中的commitRoot方法处打断点，然后点击元素，可看到未更新视图
```

​	

​    scheduleUpdateOnFiber是要在fiber上调度update，update是什么？我们去react-dom中查找方法dispatchAction，dispatchAction是什么？其实就是我们使用useState返回的第二个参数,本次的例子中特指的是updateNum。

```javascript
// 为了验证，可在source页签下，找到react-domreact-dom.development.js,在其中的dispatchAction中打断点，点击元素触发视图更新时，命中断点，然后在call Stack中找到上层函数，它就是increment作用域中的updateNum方法，所以updateNum内部其实调用的就是dispatchAction。
// 其内部做了哪些事暂且不管，发现有变量名为update，update会被赋给fiber，每个reactElement都会对应一个fiber，这个fiber对应的就是App,这个函数最终调用的是哪个方法呢？17163行，var root = scheduleUpdateOnFiber(fiber, lane, eventTime);
// 熟悉的方法，也就是在点击p标签后，会触发dispatchAction方法，在这个方法里会创建一个update，然后赋给fiber，并最终调用scheduleUpdateOnFiber来调度这次更新。当进入调度时，会和其他需要更新的内容对比一下优先级，看谁优先级高，谁会先进入render阶段。然后一通骚操作(指dfs)之后， 就进入了commitRoot方法,
// 此时的入参root，也就是整个应用的根节点，root.current也就是当前应用的根节点，也就是我们ReactDOM.render的根节点，在这个root.current上有个属性firstEffect，这个firstEffect就是本次更新需要执行DOM操作的节点有哪些，这是一个链表，这个节点有个属性叫做nextEffect会指向下一个需要更新的节点。
// 最终会把这条链表交给commit阶段，commit阶段来遍历这条链表，然后就找到了对应要进行的哪些DOM操作，然后就可以执行了。然后我们就可以看到视图上的更新。

```





