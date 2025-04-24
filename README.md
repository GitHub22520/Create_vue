创建 vue3 项目
1. 基于 vue-cli 创建
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version

## 安装或者升级你的@vue/cli 
npm install -g @vue/cli

## 执行创建命令
vue create vue_test

##  随后选择3.x
##  Choose a version of Vue.js that you want to start the project with (Use arrow keys)
##  > 3.x
##    2.x

## 启动
cd vue_test
npm run serve
2. 基于 vite创建
## 1.创建命令
  npm create vue@latest

## 2.具体配置
## 配置项目名称
√ Project name: vue3_test
## 是否添加TypeScript支持
√ Add TypeScript?  Yes
## 是否添加JSX支持
√ Add JSX Support?  No
## 是否添加路由环境
√ Add Vue Router for Single Page Application development?  No
## 是否添加pinia环境
√ Add Pinia for state management?  No
## 是否添加单元测试
√ Add Vitest for Unit Testing?  No
## 是否添加端到端测试方案
√ Add an End-to-End Testing Solution? » No
## 是否添加ESLint语法检查
√ Add ESLint for code quality?  Yes/No
## 是否添加Prettiert代码格式化
√ Add Prettier for code formatting?  No
Vue3 核心语法
【OptionsAPI 与 CompositionAPI】
● Vue2的API设计是Options（配置）风格的。
● Vue3的API设计是Composition（组合）风格的。
Options API 的弊端
Options类型的 API，数据、方法、计算属性等，是分散在：data、methods、computed中的，若想新增或者修改一个需求，就需要分别修改：data、methods、computed，不便于维护和复用。
Composition API 的优势
可以用函数的方式，更加优雅的组织代码，让相关功能的代码更加有序的组织在一起。
setup
setup 概述
setup是Vue3中一个新的配置项，值是一个函数，它是 Composition API “表演的舞台”，组件中所用到的：数据、方法、计算属性、监视......等等，均配置在setup中。
特点如下：
● setup函数返回的对象中的内容，可直接在模板中使用。
● setup中访问this是undefined。
● setup函数会在beforeCreate之前调用，它是“领先”所有钩子执行的。
<template>
    <div class="person">
        <h2>姓名：{{ name }}</h2>
        <h2>年龄：{{ age }}</h2>
        <button @click="changeName">修改名字</button>
        <button @click="changeAge">修改年龄</button>
        <button @click="showTel">查看联系方式</button>
    </div>
</template>
<script lang="ts">
export default {
    name: 'person',
    setup(props) {
        //数据 (不是响应式的)
        let name = '张三'
        let age = 18
        let tel = '12345678901'
        //响应式数据
        // let name = ref('张三')
        // let age = ref(18)
        // let tel = ref('12345678901')

        //方法
        //setup中定义的方法不能直接使用this
        function changeName() {
            name = '李四'
        }
        function changeAge() {
            age += 1
        }
        function showTel() {
            alert(tel)
        }

        return {
            name,
            age,
            changeName,
            changeAge,
            showTel
        }
    }
}
</script>
setup 的返回值
● 若返回一个对象：则对象中的：属性、方法等，在模板中均可以直接使用（重点关注）。
● 若返回一个函数：则可以自定义渲染内容，代码如下：
setup(){
  return ()=> '你好啊！'
}
setup 与 Options API 的关系
● Vue2 的配置（data、methos......）中可以访问到setup中的属性、方法。
  ○ 使用 this.XXX 指向 setup 中的 XXX数据
● 但在setup中不能访问到Vue2的配置（data、methos......）。
● 如果与Vue2冲突，则setup优先。
setup 语法糖
<template>
  <div class="person">
    <h2>姓名：{{name}}</h2>
    <h2>年龄：{{age}}</h2>
    <button @click="changName">修改名字</button>
    <button @click="changAge">年龄+1</button>
    <button @click="showTel">点我查看联系方式</button>
  </div>
</template>

<script lang="ts">
  export default {
    name:'Person',
  }
</script>

<!-- 下面的写法是setup语法糖 -->
<script setup lang="ts">
  console.log(this) //undefined
  
  // 数据（注意：此时的name、age、tel都不是响应式数据）
  let name = '张三'
  let age = 18
  let tel = '13888888888'

  // 方法
  function changName(){
    name = '李四'//注意：此时这么修改name页面是不变化的
  }
  function changAge(){
    console.log(age)
    age += 1 //注意：此时这么修改age页面是不变化的
  }
  function showTel(){
    alert(tel)
  }
</script>
【ref 创建：基本类型的响应式数据】
● 作用：定义响应式变量。
● 语法：let xxx = ref(初始值)。
● 返回值：一个RefImpl的实例对象，简称ref对象或ref，ref对象的value属性是响应式的。
● 注意点：
  ○ tS中操作数据需要：xxx.value，但模板中不需要.value，直接使用即可。
  ○ 何时需要.value？模板中不需要；包裹在响应式对象里面的ref不需要；未包裹的ref需要。
  ○ 对于let name = ref('张三')来说，name不是响应式的，name.value是响应式的。
<template>
  <div class="person">
    <h2>姓名：{{name}}</h2>
    <h2>年龄：{{age}}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">年龄+1</button>
    <button @click="showTel">点我查看联系方式</button>
  </div>
</template>

<script setup lang="ts" name="Person">
  import {ref} from 'vue'
  // name和age是一个RefImpl的实例对象，简称ref对象，它们的value属性是响应式的。
  let name = ref('张三')
  let age = ref(18)
  // tel就是一个普通的字符串，不是响应式的
  let tel = '13888888888'

  function changeName(){
    // JS中操作ref对象时候需要.value，模版中不需要手动写.value
    name.value = '李四'
    console.log(name.value)

    // 注意：name不是响应式的，name.value是响应式的，所以如下代码并不会引起页面的更新。
    // name = ref('zhang-san')
  }
  function changeAge(){
    // JS中操作ref对象时候需要.value
    age.value += 1 
    console.log(age.value)
  }
  function showTel(){
    alert(tel)
  }
</script>
reactive 创建：对象类型的响应式数据】
● 作用：定义一个响应式对象（基本类型不要用它，要用ref，否则报错）
● 语法：let 响应式对象= reactive(源对象)。
● 返回值：一个Proxy的实例对象，简称：响应式对象。
● 注意点：reactive定义的响应式数据是“深层次”的。
<template>
    <div class="person">
        <h2>一辆{{ car.brand }}车，价值{{ car.price }}万</h2>
        <button @click="changePrice">修改价格</button>
    </div>
    <br>
    <h2>游戏列表:</h2>
    <ul>
        <li v-for="game in games" :key="game.id">{{ game.name }}的价值是{{ game.price }}元</li>
    </ul>
    <button @click="changeFirstGame">修改第一个游戏的名字</button>
    <br>
    <h2>测试：{{ obj.a.b.c.d }}</h2>
    <button @click="changeObj">测试</button>
</template>
<script setup lang="ts" name="Person">
import { reactive } from 'vue';
//数据
let car = reactive({
    brand: 'Ford',
    price: 100
})
//数组
let games = reactive([
    { id: 1, name: 'GTA5', price: 200 },
    { id: 2, name: 'LOL', price: 100 },
    { id: 3, name: 'DOTA2', price: 50 }
])
//对象==>响应式对象：reactive(对象)
let obj = reactive({
    a: {
        b: {
            c: {
                d: 666
            }
        }
    }
})
//方法
function changePrice() {
    car.price += 100
}
function changeFirstGame() {
    games[0].name = '原神'
}
function changeObj() {
    obj.a.b.c.d = 888
}
</script>
【ref 创建：对象类型的响应式数据】
● 其实ref接收的数据可以是：基本类型、对象类型。
● 若ref接收的是对象类型，内部其实也是调用了reactive函数。
<template>
    <div class="person">
        <h2>一辆{{ car.brand }}车，价值{{ car.price }}万</h2>
        <button @click="changePrice">修改价格</button>
    </div>
    <br>
    <h2>游戏列表:</h2>
    <ul>
        <li v-for="game in games" :key="game.id">{{ game.name }}的价值是{{ game.price }}元</li>
    </ul>
    <button @click="changeFirstGame">修改第一个游戏的名字</button>

</template>
<script setup lang="ts" name="Person">
import { ref } from 'vue';
//数据
let car = ref({
    brand: 'Ford',
    price: 100
})
console.log(car)
//数组
let games = ref([
    { id: 1, name: 'GTA5', price: 200 },
    { id: 2, name: 'LOL', price: 100 },
    { id: 3, name: 'DOTA2', price: 50 }
])

//方法
function changePrice() {
    car.value.price += 100
}
function changeFirstGame() {
    games.value[0].name = '原神'
}

</script>
【ref 对比 reactive】
宏观角度看：
1. ref用来定义：基本类型数据、对象类型数据；
2. reactive用来定义：对象类型数据。
● 区别：
1. ref创建的变量必须使用.value（可以使用volar插件自动添加.value）。
2. reactive重新分配一个新对象，会失去响应式
  a. （可以使用Object.assign去整体替换）。 Object.assign(car, { brand: '奥拓', price: 1 })
● 使用原则：
1. 若需要一个基本类型的响应式数据，必须使用ref。
2. 若需要一个响应式对象，层级不深，ref、reactive都可以。
3. 若需要一个响应式对象，且层级较深，推荐使用reactive。
toRefs 与 toRef
● 作用：将一个响应式对象中的每一个属性，转换为ref对象。并且改变解构的值，也会影响到原响应式对象的值。
● 备注：toRefs与toRef功能一致，但toRefs可以批量转换。
<template>
    <div class="person">
        <h2>姓名：{{ person.name }}</h2>
        <h2>年龄：{{ person.age }}</h2>
        <button @click="changeName">修改姓名</button>
        <button @click="changeAge">修改年龄</button>
    </div>

</template>
<script setup lang="ts" name="Person">
import { reactive, toRefs } from 'vue';
// 响应式数据
let person = reactive({
    name: '张三',
    age: 20,
});
// 响应式数据解构
// 这里的toRefs是将响应式对象转换为ref定义的响应式数据
let { name, age } = toRefs(person);
//方法
function changeName() {
    name.value += '~';
}
function changeAge() {
    age.value += 1;
}   
</script>
计算属性
作用：根据已有数据计算出新数据（和Vue2中的computed作用一致）。
实现同样的功能，方法function没有缓存，模板调用几次，函数就执行几次；计算属性computed有缓存，模板调用多次，实际上只执行一次。
计算属性实际上是一个ref响应式对象，因此赋值时候需要加上.value
<template>
    <div class="person">
        姓：<input type="text" v-model="xing" /><br>
        名：<input type="text" v-model="ming" /><br>
        全名：<span>{{ fullname }}</span><br>
        <button @click="changeFullname">修改全名</button>
    </div>

</template>
<script setup lang="ts" name="Person">
import { computed, ref } from 'vue';
let xing = ref('张');
let ming = ref('三');
// 计算属性:有缓存
// 这样定义是只读的，不可修改
// let fullname = computed(() => {
//     return xing.value + ming.value;
// })

// 计算属性，可读可写
let fullname = computed({
    get() {
        return xing.value + ming.value;
    },
    set(value) {
        // let arr = value.split('');
        // xing.value = arr[0];
        // ming.value = arr[1];
        // 也可以用解构赋值
        [xing.value, ming.value] = value.split('');
    }
})

//因为前面的定义是只读的，无法为计算属性赋值，
function changeFullname() {
    fullname.value = '李四';
}

</script>
watch
● 作用：监视数据的变化（和Vue2中的watch作用一致）
● 特点：Vue3中的watch只能监视以下四种数据：
1. ref定义的数据。
2. reactive定义的数据。
3. 函数返回一个值（getter函数）。
4. 一个包含上述内容的数组。
我们在Vue3中使用watch的时候，通常会遇到以下几种情况：
情况一：监视ref定义的【基本类型】数据
直接写数据名即可，监视的是其value值的改变。
<template>
    <div class="Person">
        <h2>当前求和为：{{ sum }}</h2>
        <button @click="changeSum">修改 sum</button>
    </div>
</template>
<script setup lang="ts" name="Person">
import { ref, watch } from 'vue';
//数据
let sum = ref(0);

function changeSum() {
    sum.value++;
}
// 监听数据变化
const stopwatch = watch(sum, (newValue, oldValue) => {
    console.log('sum 发生变化了', newValue, oldValue);
    if (newValue > 10) {
        stopwatch();
    }
});
</script>
情况二：监视ref定义的【对象类型】数据
直接写数据名，监视的是对象的【地址值】，
若想监视对象内部的数据，要手动开启深度监视。
注意：
● 若修改的是ref定义的对象中的属性，newValue 和 oldValue 都是新值，因为它们是同一个对象。
● 若修改整个ref定义的对象，newValue 是新值， oldValue 是旧值，因为不是同一个对象了。
<template>
    <div class="Person">
        <h2>姓名：{{ person.name }}</h2>
        <h2>年龄：{{ person.age }}</h2>
        <button @click="changename">修改姓名</button>
        <button @click="changeage">修改年龄</button>
        <button @click="changeperson">修改</button>
    </div>
</template>
<script setup lang="ts" name="Person">
import { ref, watch } from 'vue'
//数据
let person = ref({
    name: '张三',
    age: 18,
})
//方法
function changename() {
    person.value.name += '~'
}
function changeage() {
    person.value.age += 1
}
function changeperson() {
    person.value = {
        name: '李四',
        age: 20,
    }
}
/*
  深度监听：
    watch的第一个参数是：被监视的数据
    watch的第二个参数是：监视的回调
    watch的第三个参数是：配置对象（deep、immediate等等.....）
*/
watch(
    person,
    (newValue, oldValue) => {
        console.log('personvalue发生了变化', newValue, oldValue)
    },
    {
        deep: true, //深度监听
        immediate: true, //立即执行
    },
)
</script>

情况三：监视reactive定义的【对象类型】数据
监视reactive定义的【对象类型】数据，且默认开启了深度监视，且深层监视无法关闭。
无法监视地址值，因为对象地址值没有改变，本质上assign在原对象上进行的是赋值。
newValue和oldValue值相同，都是新值，还是因为对象地址值没有改变，本质上assign在原对象上进行的是赋值。
<template>
    <div class="Person">
        <h2>姓名：{{ person.name }}</h2>
        <h2>年龄：{{ person.age }}</h2>
        <button @click="changename">修改姓名</button>
        <button @click="changeage">修改年龄</button>
        <button @click="changeperson">修改</button>
    </div>
</template>
<script setup lang="ts" name="Person">
import { reactive, watch } from 'vue'
//数据
let person = reactive({
    name: '张三',
    age: 18,
})
//方法
function changename() {
    person.name += '~'
}
function changeage() {
    person.age += 1
}
function changeperson() {
    Object.assign(person, {
        name: '李四',
        age: 20,
    })
}
//深度监听:监视 reactive定义的对象类型数据时，默认开启深度监视，无法关闭
watch(person, (newValue, oldValue) => {
    console.log('personvalue发生了变化', newValue, oldValue)
}, { deep: false }//深度监听默认开启，无法关闭
)
</script>

情况四：监视ref或reactive定义的【对象类型】数据中的某个属性
监视ref或reactive定义的【对象类型】数据中的某个属性，注意点如下：
1. 若该属性值不是【对象类型】即【基本类型】，需要写成函数形式，此时oldValue是旧值，newValue是新值。
2. 若该属性值是依然是【对象类型】，可直接编，也可写成函数，建议写成函数。
直接写：可以监视到对象内部属性a，b...的变化，但是监视不到整体的变化。整体改变时，对象地址值变化了，所以监视不到了。
写函数（不开启深度监视）：监视不到对象内部属性a，b...的变化，但是可以监视到整体的变化，函数返回值监视的是对象的地址值，改变整体是产生一个新对象，所以能监视到，并且新值是新值，旧值是旧值。（不过对象内部属性a，b...的新旧值都是新值）
写函数（开启深度监视）推荐：既能监视到对象内部属性a，b...的变化，也可以监视到整体的变化，函数返回值监视的是对象的地址值，改变整体是产生一个新对象，所以能监视到，并且新值是新值，旧值是旧值。（不过对象内部属性a，b...的新旧值都是新值）
结论：监视的要是对象里的属性，那么最好写函数式。
注意点：若是对象监视的是地址值，需要关注对象内部，需要手动开启深度监视。
<template>
    <div class="Person">
        <h2>姓名：{{ person.name }}</h2>
        <h2>年龄：{{ person.age }}</h2>
        <h2>汽车：</h2>
        <ul>
            <li v-for="(car, index) in person.car" :key="index">{{ car }}</li>
        </ul>
        <button @click="changeName">修改名字</button>
        <button @click="changeAge">修改年龄</button>
        <button @click="changeCar1">修改第一台车</button>
        <button @click="changeCar2">修改第二台车</button>
        <button @click="changeCar3">修改所有车</button>
    </div>
</template>
<script setup lang="ts" name="Person">
import { reactive, watch } from 'vue'
//数据
let person = reactive({
    name: '张三',
    age: 18,
    car: {
        c1: '奔驰',
        c2: '宝马',
    }
})
//方法
function changeName() {
    person.name += '~'
}
function changeAge() {
    person.age += 1
}
function changeCar1() {
    person.car.c1 += '~'
}
function changeCar2() {
    person.car.c2 += '~'
}
function changeCar3() {
    person.car = {
        c1: '法拉利',
        c2: '兰博基尼',
    }
}
/*
  监听
    监视基本类型时，需要写成箭头函数的形式
    监视对象时，可以不写成箭头函数，但不建议
*/
watch(() => person.name, (newName, oldName) => {
    console.log('名字发生了变化', newName, oldName)
})
watch(() => person.age, (newAge, oldAge) => {
    console.log('年龄发生了变化', newAge, oldAge)
})
  
watch(() => person.car, (newCar, oldCar) => {
    console.log('汽车发生了变化', newCar, oldCar)
}, { deep: true })
</script>

情况五：监视上述的多个数据
watch([() => person.name, () => person.car], (newName, oldName) => {
    console.log('名字发生了变化', newName, oldName)
}, {
    deep: true,
    immediate: true,
})
watchEffect
● 官网：立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行该函数。
● watch对比watchEffect
1. 都能监听响应式数据的变化，不同的是监听数据变化的方式不同
2. watch：要明确指出监视的数据
3. watchEffect：不用明确指出监视的数据（函数中用到哪些属性，那就监视哪些属性）。
<template>
    <div class="Person">
        <h2>当前水温：{{ temp }} ℃</h2>
        <h2>当前水位：{{ height }}cm</h2>
        <button @click="changetemp">水温+10</button>
        <button @click="changeheight">水位+10</button>
    </div>
</template>

<script setup lang="ts" name="Person">
import { ref, watch, watchEffect } from 'vue';
let temp = ref(10);
let height = ref(0);
function changetemp() {
    temp.value += 10
}
function changeheight() {
    height.value += 10
}
// 监听数据变化
// watch([temp, height], (value) => {
//     console.log(value);
//     let [temp, height] = value;
//     if (temp >= 60 || height >= 80) {
//         alert('水温过高或水位过高，请注意安全！');
//     }
// })

watchEffect(() => {
    if (temp.value >= 60 || height.value >= 80) {
        console.log('水温过高或水位过高，请注意安全！');
    }
})
</script>

【标签的 ref 属性】
作用：用于注册模板引用。
● 用在普通DOM标签上，获取的是DOM节点。
● 用在组件标签上，获取的是组件实例对象。
用在普通DOM标签上：
<template>
  <div class="person">
    <h1 ref="title1">尚硅谷</h1>
    <h2 ref="title2">前端</h2>
    <h3 ref="title3">Vue</h3>
    <input type="text" ref="inpt"> <br><br>
    <button @click="showLog">点我打印内容</button>
  </div>
</template>

<script lang="ts" setup name="Person">
  import {ref} from 'vue'
	
  let title1 = ref()
  let title2 = ref()
  let title3 = ref()

  function showLog(){
    // 通过id获取元素
    const t1 = document.getElementById('title1')
    // 打印内容
    console.log((t1 as HTMLElement).innerText)
    console.log((<HTMLElement>t1).innerText)
    console.log(t1?.innerText)
    
		/************************************/
		
    // 通过ref获取元素
    console.log(title1.value)
    console.log(title2.value)
    console.log(title3.value)
  }
</script>
用在组件标签上：
<!-- 父组件App.vue -->
<template>
  <Person ref="ren"/>
  <button @click="test">测试</button>
</template>

<script lang="ts" setup name="App">
  import Person from './components/Person.vue'
  import {ref} from 'vue'

  let ren = ref()

  function test(){
    console.log(ren.value.name)
    console.log(ren.value.age)
  }
</script>


<!-- 子组件Person.vue中要使用defineExpose暴露内容 -->
<script lang="ts" setup name="Person">
  import {ref,defineExpose} from 'vue'
	// 数据
  let name = ref('张三')
  let age = ref(18)
  /****************************/
  /****************************/
  // 使用defineExpose将组件中的数据导出交给外部
  defineExpose({name,age})
</script>
TS
// 定义一个接口，用于限制person对象的具体属性
// 这样只能一个对象使用
export interface PersonInter {
  id : string,
  name : string,
  age : number
} // 分别暴露

// 定义一个自定义类型Persons
export type Persons = Array<PersonInter>
在App.vue中使用：
<template>
  <!-- 
    :list在传递时会进行渲染
  -->
	<Person :list="persons"/> 
</template>

<script lang="ts" setup name="App">
  
import {type PersonsInter} from './types'
import {type Persons} from './types'

let person:PersonInter = {id:'111', name:'张三', age:18}

// let personList = Array<Persons>([
//    {id:'e98219e12',name:'张三',age:18},
//    {id:'e98219e13',name:'李四',age:19},
//    {id:'e98219e14',name:'王五',age:20}
// ])
  
let personList = reactive<Persons>([
   {id:'e98219e12',name:'张三',age:18},
   {id:'e98219e13',name:'李四',age:19},
   {id:'e98219e14',name:'王五',age:20}
])
Props
<template>
<div class="person">
<ul>
  <li v-for="item in list" :key="item.id">
     {{item.name}}--{{item.age}}
   </li>
 </ul>
</div>
</template>

<script lang="ts" setup name="Person">
// import {defineProps} from 'vue'  可以不用引入
import {type PersonInter} from '@/types'
import {type Persons} from '@/types'

// 第一种写法：接收 + 使用props变量保存起来
// const props = defineProps(['list'])
// console.log(list)

// 第二种写法：接收 + 限制类型
// defineProps<{list:Persons}>()

// 第三种写法：接收 + 限制类型 + 指定默认值 + 限制必要性
  // list?:Persons  ==>  父组件可传可不传list
  // withDefaults 配置默认值
let props = withDefaults(defineProps<{list?:Persons}>(),{
  list:()=>[{id:'asdasg01',name:'小猪佩奇',age:18}]
})
  
console.log(props)
</script>
【生命周期】
● 概念：Vue组件实例在创建时要经历一系列的初始化步骤，在此过程中Vue会在合适的时机，调用特定的函数，从而让开发者有机会在特定阶段运行自己的代码，这些特定的函数统称为：生命周期钩子
● 规律：
生命周期整体分为四个阶段，分别是：创建、挂载、更新、销毁，每个阶段都有两个钩子，一前一后。
● Vue2的生命周期
创建阶段：beforeCreate、created
挂载阶段：beforeMount、mounted
更新阶段：beforeUpdate、updated
销毁阶段：beforeDestroy、destroyed
● Vue3的生命周期
创建阶段：setup
挂载阶段：onBeforeMount、onMounted
更新阶段：onBeforeUpdate、onUpdated
卸载阶段：onBeforeUnmount、onUnmounted
● 常用的钩子：onMounted(挂载完毕)、onUpdated(更新完毕)、onBeforeUnmount(卸载之前)
【自定义hook】
● 什么是hook？—— 本质是一个函数，把setup函数中使用的Composition API进行了封装，类似于vue2.x中的mixin。
● 自定义hook的优势：复用代码, 让setup中的逻辑更清楚易懂。
● useSum.ts中内容如下：
import {ref,onMounted} from 'vue'

export default function(){
  let sum = ref(0)

  const increment = ()=>{
    sum.value += 1
  }
  const decrement = ()=>{
    sum.value -= 1
  }
  onMounted(()=>{
    increment()
  })

  //向外部暴露数据
  return {sum,increment,decrement}
}		
● useDog.ts中内容如下：
import {reactive,onMounted} from 'vue'
import axios,{AxiosError} from 'axios'

export default function(){
  let dogList = reactive<string[]>([])

  // 方法
  async function getDog(){
    try {
      // 发请求
      let {data} = await axios.get('https://dog.ceo/api/breed/pembroke/images/random')
      // 维护数据
      dogList.push(data.message)
    } catch (error) {
      // 处理错误
      const err = <AxiosError>error
      console.log(err.message)
    }
  }

  // 挂载钩子
  onMounted(()=>{
    getDog()
  })
    
  //向外部暴露数据
  return {dogList,getDog}
}
● 组件中具体使用：
<template>
  <h2>当前求和为：{{sum}}</h2>
  <button @click="increment">点我+1</button>
  <button @click="decrement">点我-1</button>
  <hr>
  <img v-for="(u,index) in dogList.urlList" :key="index" :src="(u as string)"> 
  <span v-show="dogList.isLoading">加载中......</span><br>
  <button @click="getDog">再来一只狗</button>
</template>

<script lang="ts">
  import {defineComponent} from 'vue'

  export default defineComponent({
    name:'App',
  })
</script>

<script setup lang="ts">
  import useSum from './hooks/useSum'
  import useDog from './hooks/useDog'
    
  let {sum,increment,decrement} = useSum()
  let {dogList,getDog} = useDog()
</script>

路由
对路由的理解
1. 路由就是一组 key-value的对应关系
2. 多个路由，需要经过路由器的管理
基本切换效果
● Vue3中要使用vue-router的最新版本，目前是4版本。
● 路由配置文件@/pages/Home.vue代码如下：
<template>
    <div class="home">
        <img src="http://www.atguigu.com/images/index_new/logo.png" alt="">
    </div>
</template>

<script setup lang="ts" name="Home">

</script>

<style scoped>
.home {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
}
</style>
● 路由配置文件@/pages/News.vue代码如下：
<template>
    <div class="news">
        <ul>
            <li><a href="#">新闻001</a></li>
            <li><a href="#">新闻002</a></li>
            <li><a href="#">新闻003</a></li>
            <li><a href="#">新闻004</a></li>
        </ul>
    </div>
</template>

<script setup lang="ts" name="News">

</script>

<style scoped>
/* 新闻 */
.news {
    padding: 0 20px;
    display: flex;
    justify-content: space-between;
    height: 100%;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: all 0.3s ease;
}

.news:hover {
    transform: translateY(-5px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.news-title {
    font-size: 20px;
    font-weight: bold;
    color: #333;
    margin-bottom: 10px;
}

.news-description {
    font-size: 14px;
    color: #666;
    line-height: 1.5;
    margin-bottom: 15px;
}

.news-date {
    font-size: 12px;
    color: #999;
    text-align: right;
}
</style>
● 路由配置文件@/pages/About.vue代码如下：
<template>
    <div class="about">
        <h2>大家好，欢迎来到尚硅谷直播间</h2>
    </div>
</template>

<script setup lang="ts" name="About">

</script>

<style scoped>
.about {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    color: rgb(85, 84, 84);
    font-size: 18px;
}
</style>
● 路由配置文件router/index.ts代码如下：
import {createRouter,createWebHistory} from 'vue-router'
import Home from '@/pages/Home.vue'
import News from '@/pages/News.vue'
import About from '@/pages/About.vue'

const router = createRouter({
    history:createWebHistory(),
    routes:[
        {
            path:'/home',
            component:Home
        },
        {
            path:'/about',
            component:About
        }
    ]
})
export default router
● main.ts代码如下：
import router from './router/index'
app.use(router)

app.mount('#app')
● App.vue代码如下
<template>
  <div class="app">
    <h2 class="title">Vue路由测试</h2>
    <!-- 导航区 -->
    <div class="navigate">
      <RouterLink to="/home" active-class="active">首页</RouterLink>
      <RouterLink to="/news" active-class="active">新闻</RouterLink>
      <RouterLink to="/about" active-class="active">关于</RouterLink>
    </div>
    <!-- 展示区 -->
    <div class="main-content">
      <RouterView></RouterView>
    </div>
  </div>
</template>

<script lang="ts" setup name="App">
  import {RouterLink,RouterView} from 'vue-router'  
</script>

【两个注意点】
1. 路由组件通常存放在pages 或 views文件夹，一般组件通常存放在components文件夹。
2. 通过点击导航，视觉效果上“消失” 了的路由组件，默认是被卸载掉的，需要的时候再去挂载。
● 路由组件：靠路由规则渲染出来的。route:[{path:/demo,component:demo}]
● 一般组件：亲手写出来的标签。<demo/>
【路由器工作模式】
1. history模式
优点：URL更加美观，不带有#，更接近传统的网站URL。
缺点：后期项目上线，需要服务端配合处理路径问题，否则刷新会有404错误。
各版本：
vue2——mode:'history'
vue3——history:createWebHistory()
React——<BrowserRouter>
const router = createRouter({
    history:createWebHistory(), //history模式
    /******/
})
2. hash模式
优点：兼容性更好，因为不需要服务器端处理路径。
缺点：URL带有#不太美观，且在SEO优化方面相对较差。
const router = createRouter({
    history:createWebHashHistory(), //hash模式
    /******/
})
【to的两种写法】
<!-- 第一种：to的字符串写法 -->
<router-link active-class="active" to="/home">主页</router-link>
<!-- 第二种：to的对象写法 -->
<router-link active-class="active" :to="{path:'/home'}">Home</router-link>
命名路由】
作用：可以简化路由跳转及传参（后面就讲）。
给路由规则命名：
routes:[
  {
    name:'zhuye',
    path:'/home',
    component:Home
  },
  {
    name:'xinwen',
    path:'/news',
    component:News,
  },
  {
    name:'guanyu',
    path:'/about',
    component:About
  }
]
跳转路由：
<!--简化前：需要写完整的路径（to的字符串写法） -->
<!--to写法(通过路径）-->
<router-link to="/news/detail">跳转</router-link>
<!--简化后：直接通过名字跳转（to的对象写法配合name属性） -->
<!--:to写法(通过名字）-->
<router-link :to="{name:'guanyu'}">跳转</router-link>
<!--:to写法(通过路径）-->
<router-link :to="{path:'/about'}">跳转</router-link>
【嵌套路由】
1. 编写News的子路由：Detail.vue
2. 配置路由规则，使用children配置项：
const router = createRouter({
  history:createWebHistory(),
    routes:[
        {
            name:'zhuye',
            path:'/home',
            component:Home
        },
        {
            name:'xinwen',
            path:'/news',
            component:News,
            children:[
                {
                    name:'xiang',
                    path:'detail',
                    component:Detail
                }
            ]
        },
        {
            name:'guanyu',
            path:'/about',
            component:About
        }
    ]
})
export default router
3. 跳转路由（记得要加完整路径）：
<router-link to="/news/detail">xxxx</router-link>
<!-- 或 -->
<router-link :to="{path:'/news/detail'}">xxxx</router-link>

4. 记得去Home组件中预留一个<router-view>
<template>
  <div class="news">
    <nav class="news-list">
      <RouterLink v-for="news in newsList" :key="news.id" :to="{path:'/news/detail'}">
        {{news.name}}
      </RouterLink>
    </nav>
    <div class="news-detail">
      <RouterView/>
    </div>
  </div>
</template>

【路由传参】
query参数
1. 传递参数
<!-- 跳转并携带query参数（to的字符串写法） -->
<router-link to="/news/detail?a=1&b=2&content=欢迎你">
    跳转
</router-link>
                
<!-- 跳转并携带query参数（to的对象写法） -->
<RouterLink 
  :to="{
    //name:'xiang', //用name也可以跳转
    path:'/news/detail',
    query:{
      id:news.id,
      title:news.title,
      content:news.content
    }
  }"
>
  {{news.title}}
</RouterLink>

2. 接收参数：
import {useRoute} from 'vue-router'
const route = useRoute()
// 打印query参数
console.log(route.query)

params参数
1. 传递参数
<!-- 跳转并携带params参数（to的字符串写法） -->
<RouterLink :to="`/news/detail/001/新闻001/内容001`">{{news.title}}</RouterLink>
                
<!-- 跳转并携带params参数（to的对象写法） -->
<RouterLink 
  :to="{
    name:'xiang', //用name跳转
    params:{
      id:news.id,
      title:news.title,
      content:news.title
    }
  }"
>
  {{news.title}}
</RouterLink>

2. 接收参数：
import {useRoute} from 'vue-router'
const route = useRoute()
// 打印params参数
console.log(route.params)
备注1：传递params参数时，若使用to的对象写法，必须使用name配置项，不能用path。
备注2：传递params参数时，需要提前在规则中占位，非必传配置可在字段后加?，例如：:id?/:title。
路由的props配置】
作用：让路由组件更方便的收到参数（可以将路由参数作为props传给组件）
{
    name:'xiang',
    path:'detail/:id/:title/:content',
    component:Detail,

  // props的对象写法，作用：把对象中的每一组key-value作为props传给Detail组件
  // props:{a:1,b:2,c:3}, 

  // props的布尔值写法，作用：把收到了每一组params参数，作为props传给Detail组件
  // props:true
  
  // props的函数写法，作用：把返回的对象中每一组key-value作为props传给Detail组件
  props(route){
    return route.query
  }
}
【 replace属性】
1. 作用：控制路由跳转时操作浏览器历史记录的模式。
2. 浏览器的历史记录有两种写入方式：分别为``push``和``replace``：
  ○ ``push``是追加历史记录（默认值）。
  ○ replace是替换当前记录。
3. 开启replace模式：
<RouterLink replace .......>News</RouterLink>
【编程式导航】
脱离 <RouterLink> 实现跳转
路由组件的两个重要的属性：$route和$router变成了两个hooks
<template>
    <div class="news">
        <!-- 导航区 -->
        <ul class="news-navigation">
            <li v-for="item in newsList" :key="item.id">
                <button @click="showNewsDetail(item)">查看新闻</button>
                <RouterLink :to="{
                    name: 'xiangqing',
                    params: {
                        id: item.id,
                        title: item.title,
                        content: item.content
                    }
                }">
                    {{ item.title }}
                </RouterLink>
            </li>
        </ul>
        <!-- 展示区 -->
        <div class="news-content">
            <RouterView></RouterView>
        </div>
    </div>
</template>

import { RouterView, RouterLink, useRouter } from 'vue-router';

const router = useRouter();

function showNewsDetail(item: any) {
    router.replace({
        name: 'xiangqing',
        params: {
            id: item.id,
            title: item.title,
            content: item.content
        }
    }) // 字符串/对象==> 用法和 to 相同
}
重定向】
1. 作用：将特定的路径，重新定向到已有路由。
2. 具体编码：
{
    path:'/',
    redirect:'/about'
}
Pinia
知识准备
1. 集中式状态管理：redux 、vuex 、 pinia
2. 准备一个效果：

【搭建 pinia 环境】
第一步：npm install pinia
第二步：操作src/main.ts
import { createApp } from 'vue'
import App from './App.vue'

/* 引入createPinia，用于创建pinia */
import { createPinia } from 'pinia'

/* 创建pinia */
const pinia = createPinia()
const app = createApp(App)

/* 使用插件 */{}
app.use(pinia)
app.mount('#app')

【存储+读取数据】
1. Store是一个保存：状态、业务逻辑 的实体，每个组件都可以读取、写入它。
2. 它有三个概念：state、getter、action，相当于组件中的： data、 computed 和 methods。
3. 具体编码：src/store/count.ts
// 引入defineStore用于创建store
import {defineStore} from 'pinia'

// 定义并暴露一个store
export const useCountStore = defineStore('count',{
  // 动作
  actions:{},
  // 状态
  state(){
    return {
      sum:6
    }
  },
  // 计算
  getters:{}
})
4. 具体编码：src/store/talk.ts
// 引入defineStore用于创建store
import {defineStore} from 'pinia'

// 定义并暴露一个store
export const useTalkStore = defineStore('talk',{
  // 动作
  actions:{},
  // 状态
  state(){
    return {
      talkList:[
        {id:'yuysada01',content:'你今天有点怪，哪里怪？怪好看的！'},
             {id:'yuysada02',content:'草莓、蓝莓、蔓越莓，你想我了没？'},
        {id:'yuysada03',content:'心里给你留了一块地，我的死心塌地'}
      ]
    }
  },
  // 计算
  getters:{}
})
5. 组件中使用state中的数据
<template>
  <h2>当前求和为：{{ sumStore.sum }}</h2>
</template>

<script setup lang="ts" name="Count">
  // 引入对应的useXxxxxStore	
  import {useSumStore} from '@/store/sum'
  
  // 调用useXxxxxStore得到对应的store
  const sumStore = useSumStore()
</script>

<template>
    <ul>
    <li v-for="talk in talkStore.talkList" :key="talk.id">
      {{ talk.content }}
    </li>
  </ul>
</template>

<script setup lang="ts" name="Count">
  import axios from 'axios'
  import {useTalkStore} from '@/store/talk'

  const talkStore = useTalkStore()
</script>

【修改数据】(三种方式)
1. 第一种修改方式，直接修改
countStore.sum = 666
2. 第二种修改方式：批量修改
countStore.$patch({
  sum:999,
  school:'atguigu'
})
3. 第三种修改方式：借助action修改（action中可以编写一些业务逻辑）
import { defineStore } from 'pinia'

export const useCountStore = defineStore('count', {
  /*************/
  actions: {
    //加
    increment(value:number) {
      if (this.sum < 10) {
        //操作countStore中的sum
        this.sum += value
      }
    },
    //减
    decrement(value:number){
      if(this.sum > 1){
        this.sum -= value
      }
    }
  },
  /*************/
})
4. 组件中调用action即可
// 使用countStore
const countStore = useCountStore()

// 调用对应action
countStore.increment(n.value)
【storeToRefs】
● 借助storeToRefs将store中的数据转为ref对象，方便在模板中使用。
● 注意：pinia提供的storeToRefs只会将数据做转换，而Vue的toRefs会转换store中数据。
<template>
    <div class="count">
        <h2>当前求和为：{{sum}}</h2>
    </div>
</template>
<script setup lang="ts" name="Count">
  import { useCountStore } from '@/store/count'
  /* 引入storeToRefs */
  import { storeToRefs } from 'pinia'

  /* 得到countStore */
  const countStore = useCountStore()
  /* 使用storeToRefs转换countStore，随后解构 */
  const {sum} = storeToRefs(countStore)
</script>

【getters】
1. 概念：当state中的数据，需要经过处理后再使用时，可以使用getters配置。
2. 追加``getters``配置。
// 引入defineStore用于创建store
import {defineStore} from 'pinia'

// 定义并暴露一个store
export const useCountStore = defineStore('count',{
  // 动作
  actions:{
    /************/
  },
  // 状态
  state(){
    return {
      sum:1,
      school:'atguigu'
    }
  },
  // 计算
  getters:{
    bigSum:(state):number => state.sum *10,
    upperSchool():string{
      return this. school.toUpperCase()
    }
  }
})
3. 组件中读取数据：
const {increment,decrement} = countStore
let {sum,school,bigSum,upperSchool} = storeToRefs(countStore)
订阅【$subscribe】
通过 store 的 $subscribe() 方法侦听 state 及其变化
talkStore.$subscribe((mutate,state)=>{
  console.log('LoveTalk',mutate,state)
  localStorage.setItem('talk',JSON.stringify(talkList.value)) // 将列表存入本地浏览器存储
})

// 在 ts 文件中取用存到本地的 talkList 数组
state(){
  return {
    talkList : JSON.parse(localStorage.getItem('talkList') as string) || [] // 不一定能取到localStorage.getItem('talkList')，添加断言
  }
}
【store组合式写法】
import {defineStore} from 'pinia'
import axios from 'axios'
import {nanoid} from 'nanoid'
import {reactive} from 'vue'

export const useTalkStore = defineStore('talk',()=>{
  // talkList就是state
  const talkList = reactive(
    JSON.parse(localStorage.getItem('talkList') as string) || []
  )

  // getATalk函数相当于action
  async function getATalk(){
    // 发请求，下面这行的写法是：连续解构赋值+重命名
    let {data:{content:title}} = await axios.get('https://api.uomg.com/api/rand.qinghua?format=json')
    // 把请求回来的字符串，包装成一个对象
    let obj = {id:nanoid(),title}
    // 放到数组中
    talkList.unshift(obj)
  }
  return {talkList,getATalk}
})
组件通信
Vue3组件通信和Vue2的区别：
● 移出事件总线，使用mitt代替。
● vuex换成了pinia。
● 把.sync优化到了v-model里面了。
● 把$listeners所有的东西，合并到$attrs中了。
● $children被砍掉了。
【props】
概述：props是使用频率最高的一种通信方式，常用与 ：父 ↔ 子。
● 若 父传子：属性值是非函数。
● 若 子传父：属性值是函数。
父组件：
<template>
  <div class="father">
    <h3>父组件，</h3>
        <h4>我的车：{{ car }}</h4>
        <h4>儿子给的玩具：{{ toy }}</h4>
        <Child :car="car" :getToy="getToy"/>
  </div>
</template>
<script setup lang="ts" name="Father">
    import Child from './Child.vue'
    import { ref } from "vue";
    // 数据
    const car = ref('奔驰')
    const toy = ref()
    // 方法
    function getToy(value:string){
        toy.value = value
    }
</script>

子组件
<template>
  <div class="child">
    <h3>子组件</h3>
        <h4>我的玩具：{{ toy }}</h4>
        <h4>父给我的车：{{ car }}</h4>
        <button @click="getToy(toy)">玩具给父亲</button>
  </div>
</template>
<script setup lang="ts" name="Child">
    import { ref } from "vue";
    const toy = ref('奥特曼')
    
    defineProps(['car','getToy'])
</script>

【自定义事件】
1. 概述：自定义事件常用于：子 => 父。
2. 注意区分好：原生事件、自定义事件。
● 原生事件：
  ○ 事件名是特定的（click、mosueenter等等）	
  ○ 事件对象$event: 是包含事件相关信息的对象（pageX、pageY、target、keyCode）
● 自定义事件：
  ○ 事件名是任意名称
  ○ 事件对象$event: 是调用emit时所提供的数据，可以是任意类型！！！
  ○ 命名方式尽量不要驼峰式，而是采取keybab-case式，即send-toy
3. 示例：
<!--在父组件中，给子组件绑定自定义事件：-->
<Child @send-toy="saveToy"/>

<!--注意区分原生事件与自定义事件中的$event-->
<button @click="toy = $event">测试</button>

//父组件中，自定义事件被触发时所调用的函数：
function saveToy(value:string){
  console.log(value)
}
//子组件中，声明事件并触发：
const emit = defineEmits(['sent-toy'])
emit('send-toy', 具体数据)
【mitt】
概述：与消息订阅与发布（pubsub）功能类似，可以实现任意组件间通信。
安装mitt
npm i mitt
新建文件：src\utils\emitter.ts
● on	触发事件
● off        移除事件
● all.clear	移除全部事件
// 引入mitt 
import mitt from "mitt";

// 创建emitter
const emitter = mitt()

/*
  // 绑定事件
  emitter.on('abc',(value)=>{
    console.log('abc事件被触发',value)
  })
  emitter.on('xyz',(value)=>{
    console.log('xyz事件被触发',value)
  })

  setInterval(() => {
    // 触发事件
    emitter.emit('abc',666)
    emitter.emit('xyz',777)
  }, 1000);

  setTimeout(() => {
    // 清理事件
    emitter.all.clear()
  }, 3000); 
*/

// 创建并暴露mitt
export default emitter
接收数据的组件中：绑定事件、同时在销毁前解绑事件：
import emitter from "@/utils/emitter";
import { onUnmounted } from "vue";

// 绑定事件
emitter.on('send-toy',(value)=>{
  console.log('send-toy事件被触发',value)
})

onUnmounted(()=>{
  // 解绑事件
  emitter.off('send-toy')
})
【第三步】：提供数据的组件，在合适的时候触发事件
import emitter from "@/utils/emitter";

function sendToy(){
  // 触发事件
  emitter.emit('send-toy',toy.value)
}
注意这个重要的内置关系，总线依赖着这个内置关系
