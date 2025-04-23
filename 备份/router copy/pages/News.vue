<template>
    <div class="news">
        <!-- 导航区 -->
        <ul class="news-navigation">
            <li v-for="item in newsList" :key="item.id">
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

<script setup lang="ts" name="News">
import { reactive } from 'vue';
import { RouterView, RouterLink, useRoute } from 'vue-router';



const newsList = reactive([
    {
        id: 'sasdaaw001', title: '很好的抗癌食物', content: '这是一篇关于抗癌食物的文章。',
    }, {
        id: "asdasd002", title: '如何一夜暴富', content: '这篇文章告诉你如何快速致富。',
    }, {
        id: "asdasd003", title: '震惊，万万没想到', content: '令人震惊的消息！',
    },
    {
        id: "asdasd004", title: '好消息！好消息!', content: '这是一个好消息！',
    }
])
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
    flex-direction: row;
}

.news-navigation {
    margin-top: 30px;
    padding-left: 10px;
    width: 20%;
    list-style-type: none;
    padding: 0;
}

.news-navigation li {
    margin-bottom: 10px;
    font-size: 16px;
    line-height: 1.5;
}

.news-navigation a {
    color: #333;
    text-decoration: none;
}

.news-navigation a:hover {
    color: aquamarine;
}

.news-content {
    width: 75%;
    padding: 20px;
    border: 1px solid #ccc;
    margin: 20px;
}
</style>