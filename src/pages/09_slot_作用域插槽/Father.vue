<template>
  <div class="father">
    <h3>父组件</h3>
    <div class="content">
      <Game>
        <template v-slot:name="params">
          <ul>
            <li
              v-for="y in params.youxi"
              :key="y.id"
            >
              {{ y.name }}
            </li>
          </ul>
        </template>
      </Game>

      <Game>
        <!--作用域插槽名字为name-->
        <template v-slot:name="params">
          <ol>
            <li
              v-for="item in params.youxi"
              :key="item.id"
            >
              {{ item.name }}
            </li>
          </ol>
        </template>
      </Game>

      <Game>
        <!--#简写：作用域插槽默认名字为default-->
        <template #default="{ youxi }">
          <!--解构赋值{ youxi }-->
          <h3
            v-for="g in youxi"
            :key="g.id"
          >{{ g.name }}</h3>
        </template>
      </Game>

    </div>
  </div>
</template>

<script setup lang="ts" name="Father">
import Game from './Game.vue'
</script>

<style scoped>
.father {
  background-color: rgb(165, 164, 164);
  padding: 20px;
  border-radius: 10px;
}

.content {
  display: flex;
  justify-content: space-evenly;
}

img,
video {
  width: 100%;
}
</style>