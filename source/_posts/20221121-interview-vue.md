---
title: 面试准备
tags: ['工作']
categories: 工作
date: 2021-01-06
---

记录工作中想到的面试应该会的问题。

<!--more-->

## 3.Vue

### 3.1 前端如何解决跨域问题

跨域的几种方式：

1. JSONP 跨域。
2. CORS 跨域。
3. Websocket 跨域。

一般跨域是后端配置的。

前端在测试时可以在`vue.config.js`中配置代理，可以在本地调用接口。

```javascript
// vue.config.js
const isDev = process.env.VUE_APP_ENV === 'development';
const path = require('path');

module.exports = {
  publicPath: './',
  productionSourceMap: isDev,
  pages: {},
  chainWebpack: (config) => {},
  devServer: {
    proxy: {
      '/': {
        target: 'https://sspuat.taikang.com',
        changeOrigin: true,
      },
    },
  },
};
```

### 3.2 MVVM

MVVM 与 MVC 的最大区别是：**MVVM 实现了 View 和 Model 的自动同步**，也就是当 Model 的属性改变时，我们不用再自己手动操作 Dom 元素来改变 View 的显示，而是改变属性后该属性对应 View 层显示会自动改变。

MVVM 解决了：

1. MVC 中大量的 DOM 操作使页面渲染性能降低，加载速度变慢，影响用户体验的问题。
2. 每次 Module 更新数据需要用户手动更新 View 的问题。

### 3.3 vue 双向绑定原理

原理：vue 是使用**数据劫持**结合**订阅-发布模式**实现双向绑定的。通过`Object.defineProperty()`方法劫持数据对象的`get()`和`set()`方法，可以理解为，在`get()`中执行的就是订阅过程，在`set()`中执行的就是发布过程。\

订阅发布模式和观察者模式

![两种开发模式](https://dqtwdd.top/cdn/img/Publish-Observer.png)

- 订阅-发布模式
  
  ```javascript
  //定义一家猎人工会
  //主要功能包括任务发布大厅(topics)，以及订阅任务(subscribe)，发布任务(publish)
  let HunterUnion = {
    type: 'hunt',
    topics: Object.create(null),
    subscribe: function (topic, fn) {
      if (!this.topics[topic]) {
        this.topics[topic] = [];
      }
      this.topics[topic].push(fn);
    },
    publish: function (topic, money) {
      if (!this.topics[topic]) return;
      for (let fn of this.topics[topic]) {
        fn(money);
      }
    },
  };
  
  //定义一个猎人类
  //包括姓名，级别
  function Hunter(name, level) {
    this.name = name;
    this.level = level;
  }
  //猎人可在猎人工会发布订阅任务
  Hunter.prototype.subscribe = function (topic, fn) {
    console.log(
      this.level + '猎人' + this.name + '订阅了狩猎' + topic + '的任务'
    );
    HunterUnion.subscribe(topic, fn);
  };
  Hunter.prototype.publish = function (topic, money) {
    console.log(
      this.level + '猎人' + this.name + '发布了狩猎' + topic + '的任务'
    );
    HunterUnion.publish(topic, money);
  };
  
  //猎人工会走来了几个猎人
  let hunterMing = new Hunter('小明', '黄金');
  let hunterJin = new Hunter('小金', '白银');
  let hunterZhang = new Hunter('小张', '黄金');
  let hunterPeter = new Hunter('Peter', '青铜');
  
  //小明，小金，小张分别订阅了狩猎tiger的任务
  hunterMing.subscribe('tiger', function (money) {
    console.log('小明表示：' + (money > 200 ? '' : '不') + '接取任务');
  });
  hunterJin.subscribe('tiger', function (money) {
    console.log('小金表示：接取任务');
  });
  hunterZhang.subscribe('tiger', function (money) {
    console.log('小张表示：接取任务');
  });
  //Peter订阅了狩猎sheep的任务
  hunterPeter.subscribe('sheep', function (money) {
    console.log('Peter表示：接取任务');
  });
  
  //Peter发布了狩猎tiger的任务
  hunterPeter.publish('tiger', 198);
  
  //猎人们发布(发布者)或订阅(观察者/订阅者)任务都是通过猎人工会(调度中心)关联起来的，他们没有直接的交流。
  ```

- 观察者模式
  
  ```javascript
  //有一家猎人工会，其中每个猎人都具有发布任务(publish)，订阅任务(subscribe)的功能
  //他们都有一个订阅列表来记录谁订阅了自己
  //定义一个猎人类
  //包括姓名，级别，订阅列表
  function Hunter(name, level) {
    this.name = name;
    this.level = level;
    this.list = [];
  }
  Hunter.prototype.publish = function (money) {
    console.log(this.level + '猎人' + this.name + '寻求帮助');
    this.list.forEach(function (item, index) {
      item(money);
    });
  };
  Hunter.prototype.subscribe = function (targrt, fn) {
    console.log(this.level + '猎人' + this.name + '订阅了' + targrt.name);
    targrt.list.push(fn);
  };
  
  //猎人工会走来了几个猎人
  let hunterMing = new Hunter('小明', '黄金');
  let hunterJin = new Hunter('小金', '白银');
  let hunterZhang = new Hunter('小张', '黄金');
  let hunterPeter = new Hunter('Peter', '青铜');
  
  //Peter等级较低，可能需要帮助，所以小明，小金，小张都订阅了Peter
  hunterMing.subscribe(hunterPeter, function (money) {
    console.log(
      '小明表示：' + (money > 200 ? '' : '暂时很忙，不能') + '给予帮助'
    );
  });
  hunterJin.subscribe(hunterPeter, function () {
    console.log('小金表示：给予帮助');
  });
  hunterZhang.subscribe(hunterPeter, function () {
    console.log('小金表示：给予帮助');
  });
  
  //Peter遇到困难，赏金198寻求帮助
  hunterPeter.publish(198);
  
  //猎人们(观察者)关联他们感兴趣的猎人(目标对象)，如Peter，当Peter有困难时，会自动通知给他们（观察者）
  ```

二者的区别在于，观察者模式相当于是观察者实例直接订阅发布者的实例，发布者在发布后直接通知观察者。而订阅发布模式相当与有一个调度中心，发布者只将任务发布到调度中心，订阅者的信息也是直接保存在调度中心中，由调度中心发出指令。

### 3.4 vue 为什么无法监听数组/对象的变化

vue2 使用的`Object.definePropertype`方法是通过对`data`进行递归遍历实现的对数据进行监控的，如果有一个很大的数组或者对象，遍历+递归的方式进行监听会非常的损耗性能，所以作者出于性能考虑，并没有对数组和对象进行深度监控，这就导致了在 vue 中某些情况下无法监听到数组和对象的改变。

- vue 能监听数组改变的情况：
  
  1. 通过**赋值形式**改变的数组`let arr = [] arr = [1,2,3]`
  2. 通过`splice`方法改变的数组 `let arr = [1,2,3] arr.splice(0,1)`
  3. 通过`push`方法改变的数组`let arr = [1] arr.push(2)`

- vue 无法监听的情况：
  
  - 数组
    
    1. 使用索引直接改变数组的某个值`let arr = [1,2,3] arr[0] = 3`
    2. 直接改变数组长度`let arr = [1,2,3,4] arr.length = 2`
  
  - 对象
    
    1. vue 中只有在 data 中声明的对象属性是响应式的，后添加的都不是响应的。
    2. 在方法中对 data 中的对象进行增加，删除，改变，无法被 vue 监听到。
    
    ```javascript
    // data
    data () {
        return {
            obj:{
                name:'王大宝',
                age:2
            }
        }
    }
    
    // methods
    this.obj.sex = 'princess' // 非响应
    delete obj.age // 非响应
    this.obj.age = 0 // 响应，因为在data中声明了age
    this.obj.sex = 'girl' // 非响应，因为是在方法中添加的sex属性
    ```

- 无法监听情况的解决方案
  
  - 数组
    
    1. 使用`Vue.set()`
    
    ```javascript
    let arr = [1, 2, 3, 4];
    Vue.set(arr, 0, 3);
    console.log(arr); // [3,2,3,4]
    ```
    
    2. 利用临时变量传参`let a = [1,2,3,4] let b = [...a] b[0] = 10 a = b` 注意，临时变量一定要是深拷贝出来的，重新开辟内存，不然不生效的。
    
    3. 使用`split()`方法对数组进行增加或删除。
  
  - 对象
    
    1. 使用`vue.set`
    2. 使用临时变量传参，同样的，临时变量一定要深拷贝出来，重新开辟内存。

vue3 中使用了`proxy`方法代替了`Object.difinePropertype`，`Object.difinePropertype`方法的局限性在于它只能对单例属性进行监听，而`proxy`属性相当于对对象架设一层拦截，对目标对象的所有访问，都必须通过这层拦截，使用这个方法可以解决使用`Object.difinePropertype`所带来的的性能损耗问题，`proxy`的缺点是这个方法是 ES6 的方法，对旧的浏览器的支持不太好，所以在 vue2 中没有使用这种方法。

### 3.5 vue 生命周期

![LifeRound](https://dqtwdd.top/cdn/img/LifeRound.png)

#### 3.5.1 vue 生命周期

1. `beforeCreated()`
   在实例初始化，完成创建之前调用
2. `created()`
   实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
3. `beforeMount()`
   在挂载开始之前被调用。 相关的 render 函数首次被调用。
4. `mounted()`
   el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。
5. `beforeUpdate()`
   数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
6. `updated()`
   由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
   当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。
   该钩子在服务器端渲染期间不被调用。
7. `beforeDestroy()`
   实例销毁之前调用。在这一步，实例仍然完全可用。
8. `destroyed()`
   Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

#### 3.5.2 `activated`与`deactivated`

页面第一次进入，钩子的触发顺序`created -> mounted -> activated`，退出时触发`deactivated`。当再次进入（前进或者后退）时，只触发`activated`。

事件挂载的方法等，只执行一次的放在`mounted`中；组件每次进去执行的方法放在 `activated` 中， `activated` 中的不管是否需要缓存多会执行。

所以当页面设置了`keepalive`的时候，要想对页面数据进行更改，则可在`activated`中调用组件中相关的方法。调用方式和`mounted`一样。

#### 3.5.3 父子组件生命周期调用顺序

- 加载渲染过程 { } { ( ) ( ) }
  
  `父beforeCreate -> 父created -> 父beforeMount -> 子beforeCreate -> 子created -> 子beforeMount -> 子mounted -> 父mounted`

- 子组件更新过程 { ( ) }
  
  `父beforeUpdate -> 子beforeUpdate -> 子updated -> 父updated`

- 父组件更新过程 { }
  
  `父beforeUpdate -> 父updated`

- 销毁过程 { ( ) }
  
  `父beforeDestroy -> 子beforeDestroy -> 子destroyed -> 父destroyed`

练习：祖父 -> 父 -> 子

- 加载渲染过程 { } { [ ] [ ( ) ( ) ] }
- 子更新过程 { [ ( ) ] }
- 销毁过程 { [ ( ) ] }

### 3.6 filter、computed 和 watch

- watch 监控现有的属性,computed 通过现有的属性计算出一个新的属性。
- watch 不会缓存数据，每次打开页面都会重新加载一次。
- computed 会缓存数据，所以 computed 的性能比 watch 更好一些。

#### 3.6.1 filter

filter 无缓存，调用频率高，因此比较适合在格式化输出的场景，主要在模板渲染时使用，主要作用是在数据展示之前对数据进行处理，让展示的数据符合我们的需求。filter 支持链式调用。

比如：将要展示的字段全部小写/大写；把要展示的数字四舍五入/保留两位小数等；为字段添加单位等等。

示例如下：

```vue
<div>
    {{ item.age | ageFilter }}
</div>
<script>
export default {
  filters: {
    ageFilter: function (item) {
      return item + '岁';
    },
  },
};
</script>
```

#### 3.6.2 computed

computed 属性具有缓存能力，在组件内普适性更强，因此适用于复杂的数据转换、统计等场景。

```vue
<template>
  <div id="app">
    <div id="nav">
      <div @click="addAge" v-for="(item, index) in listComputed" :key="index">
        {{ item.age | ageFilter }}
        {{ item.age | ageFilter }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  computed: {
    listComputed: function () {
      let newArr = this.list.filter((item) => {
        return item.age > 3;
      });
      return newArr;
    },
  },
};
</script>
```

#### 3.6.3 watch

watch 主要用于监听某个 vue 中定义的属性， 一旦属性发生了改变就去自动调用对应的方法 。

### 3.7 路由守卫

#### 3.7.1 全局守卫

全局守卫包含**beforeEach、beforeResolve、afterEach**。

`beforeResolve`在`afterEach`之前调用。

`afterEach`在进入路由后调用。

全局守卫定义在`router.js`中，路由中的每一个组件都会通过全局守卫。可以用于权限判断等：

```javascript
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
// 以上代码实现了在所有页面进入之前判断是否有登录权限，如果没有直接跳转登录页。
```

#### 3.7.2 组件内守卫

组件内守卫包含**beforeRouterEnter、beforeRouteUpdate 、beforeRouterLeave**。

组件内守卫就是定义在组件内部的守卫，可以在`componments`中定义使用：

```javascript
beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
},
beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
},
beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
}
```

`beforeRouteUpdate `在路由跳转，但是该组件被复用时调用 (2.2+) 。

`beforeRouterLeave`通常在用户即将离开某个页面时，用来提醒用户之前的操作不会被保存。

#### 3.7.3 独享守卫

独享守卫只有**beforeEnter**。

独享守卫写在`router`的路由对象中，为跳转某个路由独享的守卫：

```vu
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

独享守卫和路由内守卫的主要区别在于组件内守卫在多个路由使用同一组件时都会调用，独享守卫只在进入某个路由时才会调用。

路由守卫调用顺序：

1. 调用**路由内守卫**离开页面的`beforeRouterLeave`
2. 调用**全局守卫**`beforeEach`
3. 如果新进入的路由组件复用的话，调用**路由内守卫**`beforeRouterUpdate`
4. 调用**独享守卫**`beforeEnter`
5. 调用**路由内守卫**`beforeRouterEnter`
6. 调用**全局守卫**`beforeResolve`
7. 调用**全局守卫**`afterEach`

### 3.8 VueX

#### 3.8.1 知识点

VueX 拥有五个属性，分别是：`state getter mutation action module`

- `state`
  
  `state`可以理解为 VueX 的仓库，所有需要全局状态管理的数据都存放在`state`中，在组件中读取`state`值的方法有两种，一种是使用`getter`，另一种是使用 VueX 的`mapState`方法。使用`mapState`的优势在于，在某个组件中需要一次性使用大量 VueX 中的数据时，我们可以直接通过这种映射的方式把所有需要的属性都取过来，而不用一个一个去取。

- `getter`
  
  可以理解为 VueX 中的计算属性，它是 VueX 对外暴露的接口，可以将`state`中的值经过处理后返回。

- `mutation`
  
  VueX 中有两种改变`store`中的值，一种是使用`this.$store.state.xxxx = 'xxx'`来改变（这种方法改变`state`中的值 dom 中可能不会同步更新），另一种是使用`mutation`改变，一般来说，VueX 中的值不允许直接改变，只能使用`mutation`中的方法改变`state`中的值，这样可以方便追踪数据流向，使用方法为在组件的方法中调用`this.$store.commit('xxx')`：
  
  ```javascript
  // store.js
  import Vue from "vue";
  import Vuex from "vuex";
  
  Vue.use(Vuex);
  
  export default new Vuex.Store({
    state: {
      daughter: "王一一",
      age: 2,
      father: "栋栋",
      mother: "董菜菜",
      toys: ["小飞机", "非洲鼓", "小钢琴"]
    },
    getters: {
      getAge: function(state) {
        return state.age + "岁";
      }
    },
    mutations: {
      addAge(state, addNum = 1) {
        state.age += addNum;
      }
    },
    actions: {
        yearGone(content, year = 1) {
        return new Promise(resolve => {
          console.log("马上要执行+1啦");
          content.commit("addAge", year);
          console.log("+1完成啦");
          resolve;
        });
    },
    modules: {}
  });
  
  // app.vue
  <template>
      <div @click="print"></div>
  </template>
  <script>
  import { mapState } from "vuex";
  export default {
    computed: {
      ...mapState(["daughter", "age"])
    },
    data() {
      return {};
    },
    methods: {
      ...mapActions(["yearGone"]),
      print() {
        this.$store.commit("addAge");
      },
      printAsync() {
        this.yearGone(2).then(() => {
          console.log("actionsssssssssssss执行完啦！");
        });
      },
    }
  };
  </script>
  // printAsync : 马上要执行+1啦 +1完成啦 actionsssssssssssss执行完啦!
  ```

- `action`
  
  `action`的主要魅力在于`action`支持异步，在某些情况下，我们需要确定`mutation`改变`state`中的值之后再进行下一步操作，这时就需要用到`actions`。

- `module`
  
  在项目开发过程中，随着功能的增加，一个`VueX`里面储存的变量可能变得非常多，此外，如果多人开发一个项目，在项目开发时可能会出现`state mutation getters actions`重名的现象，这时，我们就用到了`module`。
  
  首先要说的是，`module`可以将`VueX`分解成多个`store`：
  
  ```javascript
  import Vue from 'vue';
  import Vuex from 'vuex';
  Vue.use(Vuex);
  const father = {
    state: {
      fatherName: '栋栋',
      fatherAge: 27,
    },
    getters: {
      getFatherAge: function (state) {
        return '爸爸' + state.age + '岁';
      },
    },
    mutations: {
      fatherAddAge(state, addNum = 1) {
        state.age += addNum;
      },
    },
  };
  const mother = {
    namespaced: true,
    state: {
      name: '雨雨',
      age: 26,
    },
    getters: {
      getAge: function (state) {
        return '妈妈' + state.age + '岁';
      },
    },
    mutations: {
      addAge(state, addNum = 1) {
        state.age += addNum;
      },
    },
  };
  const daughter = {
    namespaced: true,
    state: {
      name: '一一',
      age: 2,
    },
    getters: {
      getAge: function (state) {
        return '大闺女' + state.age + '岁';
      },
    },
    mutations: {
      addAge(state, addNum = 1) {
        state.age += addNum;
      },
    },
  };
  export default new Vuex.Store({
    modules: {
      father,
      mother,
      daughter,
    },
  });
  ```
  
  上面的代码中，`state`可以使用`this.$store.father.fatherName this.$store.mother.name this.$store.daughter.name`来获取。`father`定义的属性和另外两个都不一样，所以可以在组件中直接通过`this.$store.commit('fatherAddAge') this.$store.getters.getFatherAge`来触发`father`中的方法，`mother`和`daughter`定义的属性是重复的，这时最好为`modules`添加`namespaced `属性，来区分各个包的方法，方法为`this.$store.commit('mother/addAge') this.$store.commit('daughter/addAge') this.$store.getters["mother/getAge"] this.$store.getters["daughter/getAge"]`。

- 辅助函数 （`mapState mapGetters mapActions mapMutations`）
  
  ` mapState mapGetters`写在`computed`中，` mapActions mapMutations`写在`method`中，都是相当于把要调用的值/方法直接映射在组件中。
  
  ```vue
  <template>
    <div></div>
  </template>
  <script>
  import { mapState, mapGetters, mapActions, mapMutations } from 'vuex';
  export default {
    data() {
      return {};
    },
    computed: {
      ...mapState({
        viewsCount: 'views',
      }),
      ...mapGetters({
        todosALise: 'getToDo', // getToDo 不是字符串，对应的是getter里面的一个方法名字 然后将这个方法名字重新取一个别名 todosALise
      }),
    },
    methods: {
      ...mapMutations({
        totalAlise: 'clickTotal', // clickTotal 是mutation 里的方法，totalAlise是重新定义的一个别名方法，本组件直接调用这个方法
      }),
      ...mapActions({
        blogAdd: 'blogAdd', // 第一个blogAdd是定义的一个函数别名称，挂载在到this(vue)实例上，后面一个blogAdd 才是actions里面函数方法名称
      }),
    },
  };
  </script>
  ```

#### 3.8.2 常见面试题

1. vuex 有哪几种属性？
   
   有五种，分别是 State、 Getter、Mutation 、Action、 Module

2. vuex 的 State 特性是？
   
   1. Vuex 就是一个仓库，仓库里面放了很多对象。其中 state 就是数据源存放地，对应于与一般 Vue 对象里面的 data
   2. state 里面存放的数据是响应式的，Vue 组件从 store 中读取数据，若是 store 中的数据发生改变，依赖这个数据的组件也会发生更新 三、它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性中

3. vuex 的 Getter 特性是？
   
   1. getters 可以对 State 进行计算操作，它就是 Store 的计算属性
   2. 虽然在组件内也可以做计算属性，但是 getters 可以在多组件之间复用
   3. 如果一个状态只在一个组件内使用，是可以不用 getters

4. vuex 的 Mutation 特性是？
   
   Action 类似于 mutation，不同在于：Action 提交的是 mutation，而不是直接变更状态。Action 可以包含任意异步操作

5. Vue.js 中 ajax 请求代码应该写在组件的 methods 中还是 vuex 的 actions 中？
   
   1. 如果请求来的数据是不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入 vuex 的 state 里。
   2. 如果被其他地方复用，这个很大几率上是需要的，如果需要，请将请求放入 action 里，方便复用，并包装成 promise 返回，在调用处用 async await 处理返回的数据。如果不要复用这个请求，那么直接写在 vue 文件里很方便。

6. 不用 Vuex 会带来什么问题？
   
   1. 可维护性会下降，你要想修改数据，你得维护三个地方
   2. 可读性会下降，因为一个组件里的数据，你根本就看不出来是从哪来的
   3. 增加耦合，大量的上传派发，会让耦合性大大的增加，本来 Vue 用 Component 就是为了减少耦合，现在这么用，和组件化的初衷相背。 但兄弟组件有大量通信的，建议一定要用，不管大项目和小项目，因为这样会省很多事

### 3.9 如何实现一个 vue 插件

### 3.10 vue-router 原理

首先，从在浏览器地址栏输入地址到返回页面经历以下几个步骤：

1. dns 解析服务器地址。
2. 三次握手简历 tcp 链接。
3. 发送 http 请求到服务端。
4. 服务器处理 http 请求。
5. 服务器返回请求结果。
6. 关闭 tcp 链接。
7. 浏览器解析返回结果。
8. 浏览器布局渲染页面。
- 后端路由 后端路由相当于每一次改变地址（path）之后都会重新经历上面的流程，都重新去获取页面资源并渲染页面。
- 前端路由 前端路由，特别是现在一般使用的单页面应用框架，一般只需要第一次请求页面信息，然后都是在同一个 html 中渲染不同的内容。在路由改变时，是由框架的 js 监听地址的变化，然后通过 js 给变页面内容。主要是以下一个步骤：
  1. 输入 url
  2. js 解析地址
  3. 找到地址对应的页面
  4. 行页面的 js
  5. 渲染页面

此外前端路由也可以分为两种模式，一种是 hash 模式，一种是 history 模式；两种模式最大的区别在于 history 模式的可读性更好，更美观。在使用 history 模式时需要后台设置，将所有路由默认重定向到根页面，不然就会出现 404 错误。

### 3.11 vue keep-alive 原理

首先，keep-alive 是一个抽象组件，它本身不会渲染 dom 元素，也不会出现在父组件链中，使用 keep-alive 包裹动态组件时，会缓存不活动的组件实例，而不是销毁他们。

### 3.12 vue directive (自定义指令)

自定义指令是用来操作 DOM 的，尽管 Vue 推崇数据驱动识图的理念，但并非所有的情况都适合数据驱动。自定义指定就是一种有效的补充和扩展，不紧可以用于定义任何 dom 的操作，并且是复用的。

vue 指令的钩子函数

- bind：只调用一次，指令第一次绑定到元素时调用，这里可以进行一次性的初始化设置。
- inserted：被绑定的元素插入父节点时调用（保证父节点存在，但不一定已被插入文档中）。
- update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新。
- componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
- unbind：调用一次，指令与元素解绑时调用。

钩子函数的参数：

- el：指令绑定的元素，可以用来直接操作 dom。
- binding：一个对象，包含以下 property：
  - name：指令名，不包括 v-前缀
  - value：指令的绑定值，例如：v-my-directive="1+1"中，绑定值为 2。
  - expression：字符串形式的指令表达式。例如 v-my-directive="1+1"中，指令表达式为"1+1"
  - arg：传给指令的参数，可选，例如：v-my-directive:foo 中，参数为"foo"。
  - modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为{foo:true,bar:true}。
- vnode：vue 编译生成的虚拟节点。
- oldVNode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

### 3.13 vue3相关

# 
