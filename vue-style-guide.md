
## 代码规范

#### 组件名为多个单词

##### 组件名应该始终是多个单词的，根组件 App 除外。
这样做可以避免跟现有的以及未来的 HTML 元素相冲突，因为所有的 HTML 元素名称都是单个单词的。
```
//bad
Vue.component('todo', {
  // ...
})

export default {
  name: 'Todo',
  // ...
}

//good
Vue.component('todo-item', {
  // ...
})

export default {
  name: 'TodoItem',
  // ...
}
```

#### 组件数据
##### 组件的 data 必须是一个函数。
当在组件中使用 data 属性的时候 (除了 new Vue 外的任何地方)，它的值必须是返回一个对象的函数。
```
//bad
Vue.component('some-comp', {
  data: {
    foo: 'bar'
  }
})
export default {
  data: {
    foo: 'bar'
  }
}

//good
Vue.component('some-comp', {
  data: function () {
    return {
      foo: 'bar'
    }
  }
})
// In a .vue file
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
// 在一个 Vue 的根实例上直接使用对象是可以的，
// 因为只存在一个这样的实例。
new Vue({
  data: {
    foo: 'bar'
  }
})
```
#### Prop 定义
##### Prop 定义应该尽量详细。
在你提交的代码中，prop 的定义应该尽量详细，至少需要指定其类型。
```
//bad
// 这样做只有开发原型系统时可以接受
props: ['status']

//good
props: {
  status: String
}
// 更好的做法！
props: {
  status: {
    type: String,
    required: true,
    validator: function (value) {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].indexOf(value) !== -1
    }
  }
}
```
#### 为 v-for 设置键值
##### 总是用 key 配合 v-for。
在组件上总是必须用 key 配合 v-for，以便维护内部组件及其子树的状态。甚至在元素上维护可预测的行为，比如动画中的对象固化 (object constancy)，也是一种好的做法。
```
//bad
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>
//good
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

#### 避免 v-if 和 v-for 用在一起
##### 永远不要把 v-if 和 v-for 同时用在同一个元素上。
```
//bad
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

//good
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```
#### 为组件样式设置作用域
##### 对于应用来说，顶级 App 组件和布局组件中的样式可以是全局的，但是其它所有组件都应该是有作用域的。
```
//bad
<template>
  <button class="btn btn-close">X</button>
</template>

<style>
.btn-close {
  background-color: red;
}
</style>
//good
<template>
  <button class="button button-close">X</button>
</template>

<!-- 使用 `scoped` 特性 -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>
<template>
  <button :class="[$style.button, $style.buttonClose]">X</button>
</template>

<!-- 使用 CSS Modules -->
<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>
<template>
  <button class="c-Button c-Button--close">X</button>
</template>

<!-- 使用 BEM 约定 -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>
```

#### 私有属性名
##### 在插件、混入等扩展中始终为自定义的私有属性使用 $_ 前缀。并附带一个命名空间以回避和其它作者的冲突 (比如 $_yourPluginName_)。
```
//bad
var myGreatMixin = {
  // ...
  methods: {
    update: function () {
      // ...
    }
  }
}
var myGreatMixin = {
  // ...
  methods: {
    _update: function () {
      // ...
    }
  }
}
var myGreatMixin = {
  // ...
  methods: {
    $update: function () {
      // ...
    }
  }
}
var myGreatMixin = {
  // ...
  methods: {
    $_update: function () {
      // ...
    }
  }
}

//good
var myGreatMixin = {
  // ...
  methods: {
    $_myGreatMixin_update: function () {
      // ...
    }
  }
}
```
#### 组件文件
```
//bad
Vue.component('TodoList', {
  // ...
})

Vue.component('TodoItem', {
  // ...
})

//good
components/
|- TodoList.js
|- TodoItem.js
components/
|- TodoList.vue
|- TodoItem.vue
```
#### 单文件组件文件的大小写
##### 单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。
```
//bad
components/
|- mycomponent.vue
components/
|- myComponent.vue

//good
components/
|- MyComponent.vue
components/
|- my-component.vue
```

#### 基础组件名
##### 创建一个公共组件public，放入public里面

#### 单例组件名
##### 只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。
```
//bad
components/
|- Heading.vue
|- MySidebar.vue

//good
components/
|- TheHeading.vue
|- TheSidebar.vue
```

#### 紧密耦合的组件名
##### 单独创建文件夹，父组件名命文件夹名，父组件对应的文件是index.vue，在其中创建子组件。

#### 组件名中的单词顺序
##### 组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。
```
//bad
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue

//good
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```
#### 自闭合组件
##### 在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。
```
//bad
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent></MyComponent>
<!-- 在 DOM 模板中 -->
<my-component/>

//good
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent/>
<!-- 在 DOM 模板中 -->
<my-component></my-component>
```
#### 模板中的组件名大小写
##### 对于绝大多数项目来说，在单文件组件和字符串模板中组件名应该总是 PascalCase 的——但是在 DOM 模板中总是 kebab-case 的。
```
//bad
<!-- 在单文件组件和字符串模板中 -->
<mycomponent/>
<!-- 在单文件组件和字符串模板中 -->
<myComponent/>
<!-- 在 DOM 模板中 -->
<MyComponent></MyComponent>

//good
<!-- 在单文件组件和字符串模板中 -->
<MyComponent/>
<!-- 在 DOM 模板中 -->
<my-component></my-component>
<!-- 在所有地方 -->
<my-component></my-component>
```
#### JS/JSX 中的组件名大小写
##### JS/JSX 中的组件名应该始终是 PascalCase 的，尽管在较为简单的应用中只使用 Vue.component 进行全局组件注册时，可以使用 kebab-case 字符串。
```
//bad
Vue.component('myComponent', {
  // ...
})
import myComponent from './MyComponent.vue'
export default {
  name: 'myComponent',
  // ...
}
export default {
  name: 'my-component',
  // ...
}

//good
Vue.component('MyComponent', {
  // ...
})
Vue.component('my-component', {
  // ...
})
import MyComponent from './MyComponent.vue'
export default {
  name: 'MyComponent',
  // ...
}
```
#### 完整单词的组件名
##### 组件名应该倾向于完整单词而不是缩写。
```
//bad
components/
|- SdSettings.vue
|- UProfOpts.vue

//good
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```

#### Prop 名大小写
##### 在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。
```
//bad
props: {
  'greeting-text': String
}
<WelcomeMessage greetingText="hi"/>

//good
props: {
  greetingText: String
}
<WelcomeMessage greeting-text="hi"/>
```

#### 多个特性的元素
##### 如果一个特性特别长，那么单独占一行，一般情况下，超过三个特性换行。
```
//bad
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<MyComponent foo="a" bar="b" baz="c"/>

//good
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
```

#### 模板中简单的表达式
##### 组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
```
//bad
{{
  fullName.split(' ').map(function (word) {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}

//good
<!-- 在模板中 -->
{{ normalizedFullName }}
// 复杂表达式已经移入一个计算属性
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```
#### 简单的计算属性
##### 应该把复杂计算属性分割为尽可能多的更简单的属性。
```
//bad
computed: {
  price: function () {
    var basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}

//good
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```

#### 带引号的特性值
##### 非空 HTML 特性值应该始终带引号 (单引号或双引号，选你 JS 里不用的那个)。
```
//bad
<input type=text>
<AppSidebar :style={width:sidebarWidth+'px'}>

//good
<input type="text">
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```
#### 指令缩写
##### 指令缩写 (必须用 : 表示 v-bind: 和用 @ 表示 v-on:) 

#### 组件/实例的选项的顺序
##### 组件/实例的选项应该有统一的顺序。
1. 副作用 (触发组件外的影响)
- el
2. 全局感知 (要求组件以外的知识)
- name
- parent
3. 组件类型 (更改组件的类型)
-  functional
4. 模板修改器 (改变模板的编译方式)
- delimiters
- comments
5. 模板依赖 (模板内使用的资源)
- components
- directives
- filters
6. 组合 (向选项里合并属性)
- extends
- mixins
7. 接口 (组件的接口)
- inheritAttrs
- model
- props/propsData
8. 本地状态 (本地的响应式属性)
- data
- computed
9. 事件 (通过响应式事件触发的回调)
- watch
- 生命周期钩子 (按照它们被调用的顺序)
10. 非响应式的属性 (不依赖响应系统的实例属性)
- methods
11. 渲染 (组件输出的声明式描述)
- template/render
- renderError

#### 元素特性的顺序
##### 元素 (包括组件) 的特性应该有统一的顺序。
1. 定义 (提供组件的选项)
- is
2. 列表渲染 (创建多个变化的相同元素)
- v-for
3. 条件渲染 (元素是否渲染/显示)
- v-if
- v-else-if
- v-else
- v-show
- v-cloak
4. 渲染方式 (改变元素的渲染方式)
- v-pre
- v-once
5. 全局感知 (需要超越组件的知识)
- id
6. 唯一的特性 (需要唯一值的特性)
- ref
- key
- slot
7. 双向绑定 (把绑定和事件结合起来)
- v-model
8. 其它特性 (所有普通的绑定或未绑定的特性)
9. 事件 (组件事件监听器)
- v-on
10. 内容 (覆写元素的内容)
- v-html
- v-text

#### 组件/实例选项中的空行
##### 不做强制要求，空行的话就都保持空行，不空的话就都不空，不允许出现多次空行。

#### 单文件组件的顶级元素的顺序
##### 单文件组件应该总是让 <script>、<template> 和 <style> 标签的顺序保持一致。且 <style> 要放在最后，因为另外两个标签至少要有一个。

#### 在 v-if/v-if-else/v-else 中使用 key

#### scoped 中的元素选择器
##### 元素选择器应该避免在 scoped 中出现。
```
//bad
<template>
  <button>X</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>

//good
<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```
#### 隐性的父子组件通信
##### 应该优先通过 prop 和事件进行父子组件之间的通信，而不是 this.$parent 或改变 prop。
```
//bad
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: '<input v-model="todo.text">'
})
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    removeTodo () {
      var vm = this
      vm.$parent.todos = vm.$parent.todos.filter(function (todo) {
        return todo.id !== vm.todo.id
      })
    }
  },
  template: `
    <span>
      {{ todo.text }}
      <button @click="removeTodo">
        X
      </button>
    </span>
  `
  
  //good
  Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: `
    <input
      :value="todo.text"
      @input="$emit('input', $event.target.value)"
    >
  `
})
Vue.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  template: `
    <span>
      {{ todo.text }}
      <button @click="$emit('delete')">
        X
      </button>
    </span>
  `
})
```

#### 非 Flux 的全局状态管理
##### 应该优先通过 Vuex 管理全局状态，而不是通过 this.$root 或一个全局事件总线。
```
//bad
// main.js
new Vue({
  data: {
    todos: []
  },
  created: function () {
    this.$on('remove-todo', this.removeTodo)
  },
  methods: {
    removeTodo: function (todo) {
      var todoIdToRemove = todo.id
      this.todos = this.todos.filter(function (todo) {
        return todo.id !== todoIdToRemove
      })
    }
  }
})

//good
// store/modules/todos.js
export default {
  state: {
    list: []
  },
  mutations: {
    REMOVE_TODO (state, todoId) {
      state.list = state.list.filter(todo => todo.id !== todoId)
    }
  },
  actions: {
    removeTodo ({ commit, state }, todo) {
      commit('REMOVE_TODO', todo.id)
    }
  }
}
<!-- TodoItem.vue -->
<template>
  <span>
    {{ todo.text }}
    <button @click="removeTodo(todo)">
      X
    </button>
  </span>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: mapActions(['removeTodo'])
}
</script>
```
#### 将 this 赋值给 component 变量
```
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      console.log(this.hello());
    },
  },
};
</script>

<!-- 避免 -->
<script type="text/javascript">
export default {
  methods: {
    hello() {
      return 'hello';
    },
    printHello() {
      const self = this; // 没有必要
      console.log(self.hello());
    },
  },
};
</script>
```

#### 谨慎使用 this.$refs

#### 使用组件名作为样式作用域空间
```
<style scoped>
  /* 推荐 */
  .MyExample { }
  .MyExample li { }
  .MyExample__item { }

  /* 避免 */
  .My-Example { } /* 没有用组件名或模块名限制作用域, 不符合 BEM 规范 */
</style>
```

#### 提供组件 API 文档
在模块目录中添加 README.md 文件：
```
range-slider/
├── range-slider.vue
├── range-slider.less
└── README.md
```



