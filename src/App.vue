<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from 'axios';
export default {
  components: {
    MyButton,
    MyDialog,
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      // modificatorValue: ''
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        const response = await axios.get("http://jsonplaceholder.typicode.com/posts?_limit=10");
        this.posts = response.data;
      } catch (e) {
        alert('Ошибка')
      }
    }
  }
}
</script>

<template>
  <div class="app">
    <h1>Страница с постами</h1>
<!--    <input type="text" v-model.trim="modificatorValue">-->
    <my-button @click="fetchPosts">Получить постыв</my-button>
    <my-button @click="showDialog" style="margin-top: 15px; margin-bottom: 15px">Создать пост</my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <post-list v-bind:posts="posts" @remove="removePost"/>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}
</style>

<!--Single file component-->