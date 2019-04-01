# vue-something-lifecycle
前言：

之前或多或少的在项目中使用了Vue，的确对开发很便利；

过程中一知半解的边看API，看运用到项目中，挖了很多坑，掉了很多坑；项目结束后却又发现自己还是不了解Vue，仅限于使用了；

所以患懒癌的自己开始写点东西；一些目前自己的看法，以及各位勤劳的大神分享的学习笔记总结：

API中的周期图肯定是要慢慢看懂得



觉得要学Vue，先从基本开始--Vue的生命周期:

Vue 实例 --简单来讲 从创建 到 销毁，称为一个周期。使用的是根dom元素，只读的。

过程大类包括： 创建create，挂载（载入）mount，更新update，销毁destory等
而生命周期中，细化的提供了一些生命周期的钩子函数，(周期中自动执行的函数)，可用于自定义一些逻辑；
```
<div id=showStr>{{a}}</div>
<script>

var myVue = new Vue({          

   el: "#showStr",          

   data: { a: "This is Vue"        }        

</script> 
```
1. beforeCreate():
实例初始化之后，数据的观测observer，初始化事件init event之前被调用，所以此时：
```
console.log(this.a)  \\undefined   数据        
console.log(this.$el) \\undefined   dom对象
```
2. created():
实例创建之后调用，进行 数据观测，属性与方法的运算，以及事件回调，此时挂载（载入）未开始，元素不可见：
```
console.log(this.a)  \\"This is Vue"数据        
console.log(this.$el) \\undefined   dom对象
```
3. befoeMount(): 载入之前。调用render（）等，编译模板生成html，但是未载入页面上。虚拟出一个dom来进行占位，
```
 console.log(this.a)
  \\"This is Vue"数据        
 console.log(this.$el) \\dom节点，但数据未定义{{a}} ；
```
4. mounted(): 

html的内容将会替换el属性指定的dom对象，渲染到了html页面中，mounted只会执行一次；      
```
console.log(this.$el) \\dom节点
  ，"This is Vue"数据渲染到页面中
```
（但是它不会保证 所有的子组件也都被一起挂载，若想要等到整个视图都渲染完毕，可以使用 vm.$nextTickt来内嵌掉用来替换掉它，）

5. beforeUpdate(): 

数据更新之前调用，在重新渲染之前进行调用，并且不会触发附加的重渲染过程；该钩子在服务器端渲染期间不被调用。6.updated(): 

数据更改之后，此时dom已进行更新。若此期间进行更改状态的操作，可能会导致更新无限循环， 

（但是它不会 保证 所有的子组件也都被一起挂载，若想要等到整个视图都渲染完毕，可以使用
 vm.$nextTickt来内嵌掉用来替换掉它，） 

该钩子在服务器端渲染期间不被调用。

7. keep-alive： 
 官方： https://cn.vuejs.org/v2/api/#keep-alive

 看过的某个大神的http://blog.csdn.net/github_36546211/article/details/78517496  

 包裹动态组件时，会缓存不活动的组件实例，而不是销毁。自身是一个抽象组件，不会渲染一个DOM元素，也不出现在父组件链中；
```
<!-- 基本 -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>

<!-- 多个条件判断的子组件 -->
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>

<!-- 和 `<transition>` 一起使用 -->
<transition>
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
</transition>
```
 当组件在 <keep-alive> 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。来得知当前组件是否处于活动状态
主要用于保留组件状态或避免重新渲染。  

acvitated()： 

deactivated():

8.beforeDestroy(): 

 实例销毁之前调用，所以，实例仍可用。

9.destoryed()： 

 销毁之后，所有的事件监听会被移除，子实例也会被销毁，在服务端渲染期间不被调用。

10.errorCaptured(): 2.5.0+新增
 捕获子孙组件的错误时被调用，（错误对象，发生错误的组件实例，包含错误来源信息的字符串）
