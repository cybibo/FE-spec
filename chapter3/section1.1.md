## 组件中写选项的顺序
### 副作用 (触发组件外的影响)
el
### 全局感知 (要求组件以外的知识)
name  
parent
### 组件类型 (更改组件的类型)
functional
### 模板修改器 (改变模板的编译方式)
delimiters  
comments
### 模板依赖 (模板内使用的资源)
components  
directives  
filters
### 组合 (向选项里合并属性)
extends  
mixins
### 接口 (组件的接口)
inheritAttrs  
model  
props/propsData
### 本地状态 (本地的响应式属性)
data  
computed
### 事件 (通过响应式事件触发的回调)
watch
### 生命周期钩子 (按照它们被调用的顺序)
beforeCreate  
created  
beforeMount  
mounted  
beforeUpdate  
updated  
activated  
deactivated. 
beforeDestroy  
destroyed. 
### 非响应式的属性 (不依赖响应系统的实例属性)
methods
### 渲染 (组件输出的声明式描述)
template/render. 
renderError
