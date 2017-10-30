# VUE 源码学习01 源码入口【version：2.4.1】
Vue项目做了不少，最近在学习设计模式与Vue源码，记录一下自己的脚印！共勉！注：此处源码学习方式为先了解其大模块，从宏观再去到微观学习，以免一开始就研究细节然后出不来~

#### 从package.json文件知道我们在执行命令npm run dev（只是以dev为例prod一样） 对应的是config.js文件里面的web-full-dev，然后找到config.js文件
	config.js
	'web-full-dev': {
	entry: resolve('web/entry-runtime-with-compiler.js'),
	}
    
#### 这里就能清楚的知道配置入口文件是	entry-runtime-with-compiler.js
	entry-runtime-with-compiler.js
	import config from 'core/config'
	import Vue from './runtime/index'
	export default Vue
#### 这里我就只主要写出了文件的导入和导出，这里就能知道这个文件只是为了导出一个Vue,那这个Vue具体的来源呢，继续看import的Vue的来源
	runtime/index.js
	import Vue from 'core/index'
#### 该怎样继续不用啰嗦了，继续找	core/index.js
#### 你会发现最后找到了	instance/index.js
####此时你已经找到了核心初始化的代码！
	instance/index.js
	import { initMixin } from './init'
	import { stateMixin } from './state'
	import { renderMixin } from './render'
	import { eventsMixin } from './events'
	import { lifecycleMixin } from './lifecycle'
	import { warn } from '../util/index'
	
	function Vue (options) {
	if (process.env.NODE_ENV !== 'production' &&
	!(this instanceof Vue)
	) {
	warn('Vue is a constructor and should be called with the `new` keyword')
	}
	this._init(options)
	}
	
	initMixin(Vue)
	stateMixin(Vue)
	eventsMixin(Vue)
	lifecycleMixin(Vue)
	renderMixin(Vue)
	
	export default Vue
####从代码可以看出此文件主要就是定义了一个构造函数Vue,并且整个Vue的核心方法都到场了
	1.//初始化的入口，各种初始化工作
	2.initMixin(Vue) 
	3.//数据绑定的核心方法，包括常用的$watch方法
	4.stateMixin(Vue)
	5.//事件的核心方法，包括常用的$on，$off，$emit方法
	6.eventsMixin(Vue)
	7.//生命周期的核心方法
	8.lifecycleMixin(Vue)
	9.//渲染的核心方法，用来生成render函数以及VNode
	10.renderMixin(Vue)
		
###至此，我们已经找到了核心入口文件，从这个文件的	initMixin(Vue)
###方法继续往下走可以进行下面各个功能模块探究！
