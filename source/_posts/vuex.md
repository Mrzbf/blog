---
layout: post
title: "vuex入门总结"
comments: true
tags: 
	- code 前端 
categories:
	- JavaScript vue
---
使用 Vuex 并不意味着你需要将所有的状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。
+ 项目目录结构
![image](/assets/img/js/project.png)
+ 1.state想当于一个全局变量，同一模块中的任何组件都可以访问到它的值。
+ 2.Mutation更改vuex内state中的状态，mutation中只能进行同步操作，官方文档中建议使用常量替代Mutation事件类型。
+ 3.action提交mutation不直接修改state中的状态，一般将异步操作放在action中。
+ 4.getter对state中的状态进行计算
+ 5.module,将vuex中的store分割成模块，每个模块拥有自己的state，mutation，action
+ 6.在组件中使用store，用map系列函数（mapState、mapMutations、mapActions、mapGetters）将其映射修改，就可以在组件中使用this.名称进行访问。
<br>  
### tips:
+ mapState、mapMutations、mapActions、mapGetters是vuex内的对象不能写错,在index.js中对store进行封装和导出。
+ 使用vuex的时候对状态进行权衡，不必所有的状态都放入state中。

