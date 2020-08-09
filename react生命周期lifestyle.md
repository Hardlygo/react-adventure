

 理解一个框架的生命周期很重要，知道了生命周期可以让你知道你可以在哪个生命周期阶段写上你的代码，从而触发到你想要的做出的动作，做出你想要的修改，以达到你想要的的效果。
 众所周知react跟vue很相像，vue有他的生命周期，react也有。结合之前对vue的学习以及对vue生命周期的理解，我猜想，只要找到react和vue想像的阶段，就可以做之前vue在该阶段所做的一样的事，如：在mounted阶段做请求等。
 1. vue的生命周期
 ![VUE生命周期](https://cn.vuejs.org/images/lifecycle.png "vue的生命周期")
 2. react的生命周期
 react的生命周期分两个版本
 - React16旧的生命周期
 - React16新的生命周期
 两个生命周期的区别：
 1. React16新的生命周期弃用了componentWillMount、componentWillReceiveProps，componentWillUpdate
新增了getDerivedStateFromProps、getSnapshotBeforeUpdate来代替弃用的三个钩子函数（componentWillMount、componentWillReceiveProps，componentWillUpdate）
2. React16并没有删除这三个钩子函数，但是不能和新增的钩子函数（getDerivedStateFromProps、getSnapshotBeforeUpdate）混用，React17将会删除componentWillMount、componentWillReceiveProps，componentWillUpdate
3. 新增了对错误的处理（componentDidCatch）

react旧生命周期 
1. 图一
 ![react旧生命周期](https://segmentfault.com/img/bVbhRhS "react旧的生命周期")
2. 图二
![react旧生命周期](https://segmentfault.com/img/bVbhRvE "react旧的生命周期")

react组建的生命周期总共可以分为三个阶段
1. Mounting（加载阶段，只会执行一次）
2. Updating（更新阶段，生命周期内可能会执行多次）
3. Unmounting（卸载阶段，只会执行一次）

参考：https://segmentfault.com/a/1190000016617400