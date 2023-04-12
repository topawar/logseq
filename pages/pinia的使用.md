- #vue #pinia
- 概念：Pinia 是 Vue 的专属状态管理库，它允许你跨组件或页面共享状态。
- 能够实现的功能：
	- Devtools 支持
	- 追踪 actions、mutations 的时间线
	- 在组件中展示它们所用到的 Store
	- 让调试更容易的 Time travel
	- 热更新
	- 不必重载页面即可修改 Store
	- 开发时可保持当前的 State
	- 插件：可通过插件扩展 Pinia 功能
	- 为 JS 开发者提供适当的 TypeScript 支持以及**自动补全**功能。
	- 支持服务端渲染
- ### 使用pinia
	- #### 定义store
		- Store 是用 `defineStore()` 定义的，它的第一个参数要求是一个**独一无二的**名字
		- `defineStore()` 的第二个参数可接受两类值：Setup 函数或 Option 对象。
	- ``` typescript
	  import { defineStore } from 'pinia'
	  
	  // 你可以对 `defineStore()` 的返回值进行任意命名，但最好使用 store 的名字，同时以 `use` 开头且以 `Store` 结尾。(比如 `useUserStore`，`useCartStore`，`useProductStore`)
	  // 第一个参数是你的应用中 Store 的唯一 ID。
	  export const useAlertsStore = defineStore('alerts', {
	    // 其他配置...
	  })
	  ```
	- #### Option Store
	  
	  与 Vue 的选项式 API 类似，我们也可以传入一个带有 `state`、`actions` 与 `getters` 属性的 Option 对象
	- >你可以认为 `state` 是 store 的数据 (`data`)，`getters` 是 store 的计算属性 (`computed`)，而 `actions` 则是方法 (`methods`)。
	- ``` typescript
	  export const useCounterStore = defineStore('counter', {
	    state: () => ({ count: 0 }),
	    getters: {
	      double: (state) => state.count * 2,
	    },
	    actions: {
	      increment() {
	        this.count++
	      },
	    },
	  })
	  ```
	- #### Setup Store 
	  
	  也存在另一种定义 store 的可用语法。与 Vue 组合式 API 的 [setup 函数](https://cn.vuejs.org/api/composition-api-setup.html) 相似，我们可以传入一个函数，该函数定义了一些响应式属性和方法，并且返回一个带有我们想暴露出去的属性和方法的对象。
		- #+BEGIN_IMPORTANT
		  >在 Setup Store 中：
		  >ref() 就是 state 属性
		  >computed() 就是 getters
		  >function() 就是 actions
		  >Setup store 比 Option Store 带来了更多的灵活性，因为你可以在一个 store 内创建侦听器，并自>由地使用任何组合式函数。不过，请记住，使用组合式函数会让 SSR 变得更加复杂。
		  #+END_IMPORTANT
	- ``` typescript
	  export const useCounterStore = defineStore('counter', () => {
	    const count = ref(0)
	    function increment() {
	      count.value++
	    }
	  
	    return { count, increment }
	  })
	  ```
-