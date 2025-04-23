<template>
    <ul class="news-list">
        <li>编号: {{ id }}</li>
        <li>标题: {{ title }}</li>
        <li>内容: {{ content }}</li>
    </ul>
</template>

<script setup lang="ts" name="Detail">
// import { useRoute } from 'vue-router';

// const route = useRoute();
// console.log(route);
defineProps(['id', 'title', 'content']);
</script>

<style scoped>
.news-list {
    list-style-type: none;
    padding: 0;
    font-size: 16px;
    line-height: 1.5;
}

.news-list li {
    margin-bottom: 10px;
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
}

.news-list li::before {
    content: attr(data-label);
    font-weight: bold;
    margin-right: 10px;
}
</style>