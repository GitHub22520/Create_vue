<template>
    <div class="count">
        <h2>当前求和为：{{ sum }}, 放大 10 倍后{{ bigSum }}</h2>
        <select v-model.number="n">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select>
        <button @click="add">➕</button>
        <button @click="sub">➖</button>
    </div>
</template>


<script setup lang="ts" name="Count">
import { ref } from 'vue'
import { useCountStore} from '@/store/count'
import { storeToRefs } from 'pinia'

const countStore = useCountStore()
// console.log(countStore.sum)
const {sum,bigSum} = storeToRefs(countStore)

let n = ref(1)

function add() {
    // 第一种修改方式
    // countStore.sum += n.value
    // 第二种修改
    // countStore.$patch({
    //     sum: 888
    // })
    // 第三种修改
    countStore.increment(n.value)
}
function sub() {
    countStore.sum -= n.value
}
</script>

<style scoped>
.count {
    background-color: skyblue;
    padding: 10px;
    border-radius: 20px;
    box-shadow: 0 0 10px;
}
select, button {
    margin: 0 5px;
    height: 25px;
}
</style>