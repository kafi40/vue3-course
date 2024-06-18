<script>
  import MyInput from "@/components/UI/MyInput.vue";

  export default {
    components: {MyInput},
    data() {
      return {
        post: {
          title: '',
          body: ''
        }
      }
    },
    methods: {
      createPost() {
        this.post.id = Date.now()
        this.$emit('create', this.post)
        this.post = {
          title: '',
          body: ''
        }
      }
    }
  }
</script>

<template>
  <form @submit.prevent>
    <h4>Создание поста</h4>
<!--    <input v-bind:value="post.title" @input="post.title = $event.target.value" class="input" type="text" placeholder="Название">-->
<!--    <input v-bind:value="post.body" @input="post.body = $event.target.value" class="input" type="text" placeholder="Описание">-->
<!--    <input v-model="post.title" class="input" type="text" placeholder="Название">-->
<!--    <input v-model="post.body" class="input" type="text" placeholder="Описание">-->
    <my-input v-model="post.title" type="text" placeholder="Название"/>
    <my-input v-model="post.body" type="text" placeholder="Описание"/>
    <my-button class="btn" style="align-self: flex-end; margin-top: 15px" @click="createPost">Создать</my-button>
  </form>
</template>

<style scoped>
  form {
    display: flex;
    flex-direction: column;
  }
</style>