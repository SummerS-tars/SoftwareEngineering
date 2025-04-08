# setup

---

- [1. 基础语法](#1-基础语法)
    - [1.1. **`let`**](#11-let)
    - [1.2. **`function`**](#12-function)
    - [1.3. **`return`**](#13-return)
- [2. 进阶语法](#2-进阶语法)
    - [2.1. setup语法糖](#21-setup语法糖)

---

**例子：**

```ts
import { ref } from 'vue'

export default {
    setup() {
        let count = ref(0)

        function add() {
            count.value++
        }

        return {
            count,
            add
        }
    }
}
```

---

## 1. 基础语法

### 1.1. **`let`**

- example:  

    ```js
    let count = ref(0)
    ```

### 1.2. **`function`**

- example:  

    ```js
    function add() {
        count.value++
    }
    ```

### 1.3. **`return`**

- example:  

    ```js
    return {
        count,
        add
    }
    ```

- 返回数据和方法，供模板使用  
    若没有返回，则无法被模板使用  

---

## 2. 进阶语法

### 2.1. setup语法糖

- 用来简化`setup`函数的写法  
- 例如上述的`script`的example，可以作如下简化:  

    ```ts
    <script lang="ts">
        export default {
            name: 'Example',
        }
    </script>

    <script lang="ts" setup>
        let count = ref(0)

        function add() {
            count.value++
        }
    </script>
    ```

    - `setup`函数中的变量和方法，可以直接在模板中使用  
        无需通过`return`返回  
    - 可以借助插件，将`name`也加入到`setup`所在的`script`头中  
