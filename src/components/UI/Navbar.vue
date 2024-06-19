<script>

import {defineComponent} from "vue";
import MyButton from "@/components/UI/MyButton.vue";

export default defineComponent({
  components: {MyButton}
})
</script>

<template>
  <div class="navbar">
    <div @click="$router.push('/')">Vue 3</div>
    <div class="navbar__btns">
      <my-button @click="$router.push('/posts')">Посты</my-button>
      <my-button @click="$router.push('/about')" style="margin-left: 20px">О приложении</my-button>
      <my-button @click="$router.push('/store')" style="margin-left: 20px">Store</my-button>
    </div>
  </div>
</template>

<style scoped>
  .navbar {
    height: 50px;
    background-color: lightgray;
    box-shadow: 2px 2px 4px grey;
    display: flex;
    align-items: center;
    padding: 0 15px;
  }

  .navbar__btns {
    margin-left: auto;
  }
</style>