<template>
    <div class="talk">
        <button @click="getLoveTalk">获取一句土味情话</button>
        <ul>
            <li v-for="item in talkStore.talkList" :key="item.id">{{ item.title }}</li>
        </ul>
    </div>
</template>

<script setup lang="ts" name="LoveTalk">
import { reactive } from 'vue';
import axios from 'axios';
import { nanoid } from 'nanoid';
import { useTalkStore } from '@/store/lovetalk';
import { storeToRefs } from 'pinia';

const talkStore = useTalkStore()
// console.log(talkStore.talkList)
const {talkList} = storeToRefs(talkStore)

talkStore.$subscribe((mutate,state)=>{
    console.log('talkStore 里面保存的数据发生了变化',mutate,state)
    localStorage.setItem('talkList',JSON.stringify(state.talkList))
})

function getLoveTalk(){
    talkStore.getATalk()
}

</script>

<style scoped>
.talk {
    background-color: rgb(199, 88, 207);
    padding: 10px;
    border-radius: 20px;
    box-shadow: 0 0 10px;
}
</style>