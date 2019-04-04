# Introduction

## 响应式原理

把一个普通的javascript传递给vue实例的data选项后，vue实例循环此对象的属性，并使用object.defineproperty劫持该属性的getter/setter。通过组件实例相对应的watcher，在组件渲染的过程中把属性设置为依赖，当依赖项的setter被调用时，会通知watcher重新计算，从而致使相关联的组件得以更新。

## data为什么是一个函数类型

当data为一个对象类型，组件需用被用来创建多个实例时，所有实例都会引用同一个data对象。 当data为一个函数类型，并在函数里return一个新对象，这样组件创建实例就会引用函数中return的新的对象，不会与其他实例共享。

## 什么是穿透

为了让css单独作用于组件中，使用scoped属性，可以使组件之间的样式互不干扰。但在引用第三方组件时，修改不了第三方组件的样式，需要穿透scoped。

