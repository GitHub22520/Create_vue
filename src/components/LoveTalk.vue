<template>
    <div class="talk">
        <button @click="getTalk">获取一句土味情话</button>
        <ul>
            <li v-for="item in talkList" :key="item.id">{{ item.title }}</li>
        </ul>
    </div>
</template>


<script setup lang="ts" name="LoveTalk">
import { reactive } from 'vue';
import axios from 'axios';
import { nanoid } from 'nanoid';

let talkList = reactive([
    {id:'iiiu01', title:'你是我心中的一首歌，永远也唱不腻。'},
]);

async function getTalk() {
    //连续结构赋值+重命名
    let {data:{content:title}} = await axios.get('https://api.uomg.com/api/rand.qinghua?format=json');
    let obj = {id:nanoid(), title}
    // console.log(obj);
    talkList.unshift(obj);
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