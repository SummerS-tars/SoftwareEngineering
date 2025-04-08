# `computed`属性

---

- [1. 介绍](#1-介绍)
- [2. 使用](#2-使用)
- [3. 配合使用](#3-配合使用)

---

## 1. 介绍

`computed`: 计算属性  

本质上是一个函数  
返回一个`ref`对象  
在依赖值发生变化时，会重新计算此对象  

---

## 2. 使用

- 接受参数:  
    可以认为接受的参数为方法(函数)  
    - 回显函数定义法：仅读  
        `() => {}`  
    - `get` `set` 方法：可读可写  
        `get() {}` `set() {}`  
        示例：  

        ```ts
        let fullName = computed({
            get(){
                return firstName.value + ' ' + lastName.value
            },
            set(value){
                let name = value.split(' ')
                firstName.value = name[1]
                lastName.value = name[0]
            }
        })

        function resetFullName()
        {
            nameFull.value = 'ni ge'
        }
        ```

- 返回值: `ref`对象
    - 回显函数定义法: `ComputedRef`  
    - `get` `set` 方法: `WritableComputedRef`

---

## 3. 配合使用

`input`输入框与计算属性的配合使用：  

```html
<template>
    <div>
        <input type="text" v-model="firstName"> <br>
        <button @click="resetNameFull">reset</button>
    </div>
</template>
```

上述`template`段中  

- `input`：输入框  
- `v-model="firstName"`：双向绑定  
    `firstName`为`ref`对象  
    作用是将输入框的值与`firstName`绑定  
