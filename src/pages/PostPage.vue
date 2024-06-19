<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from 'axios';
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";
export default {
  components: {
    MyInput,
    MySelect,
    MyButton,
    MyDialog,
    PostList, PostForm
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      // modificatorValue: ''
      isPostsLoading: false,
      searchQuery: '',
      selectedSort: '',
      page: 1,
      limit: 10,
      totalPage: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'}
      ]
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
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    //   // this.fetchPosts();
    // },
    async fetchPosts() {
      this.isPostsLoading = true;
      try {
        // setTimeout(async () => {
        //   const response = await axios.get("http://jsonplaceholder.typicode.com/posts?_limit=10");
        //   this.posts = response.data;
        //   this.isPostsLoading = false;
        // }, 1000)
        const response = await axios.get("http://jsonplaceholder.typicode.com/posts?_limit=10", {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = response.data;
      } catch (e) {
        alert('Ошибка')
      } finally {
        this.isPostsLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1;
        // setTimeout(async () => {
        //   const response = await axios.get("http://jsonplaceholder.typicode.com/posts?_limit=10");
        //   this.posts = response.data;
        //   this.isPostsLoading = false;
        // }, 1000)
        const response = await axios.get("http://jsonplaceholder.typicode.com/posts?_limit=10", {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        });
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = [...this.posts, ...response.data];
      } catch (e) {
        alert('Ошибка')
      } finally {
      }
    }
  },
  mounted() {
    this.fetchPosts();
    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPage) {
    //     this.loadMorePosts();
    //   }
    // };
    // const observer = new IntersectionObserver(callback, options)
    // observer.observe(this.$refs.observer);
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]));
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
    }
  },
  watch: {
    // selectedSort(newValue) {
    //   this.posts.sort((post1, post2) => {
    //     return post1[newValue]?.localeCompare(post2[newValue]);
    //   })
    // },
    // dialogVisible(newValue) {
    //
    // }
    // page() {
    //   this.fetchPosts();
    // }
  }
}
</script>

<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <my-input v-focus v-model="searchQuery" placeholder="Поиск..."/>
    <div class="app__btns">
      <my-button @click="showDialog">Создать пост</my-button>
      <my-select v-model="selectedSort" :options="sortOptions"/>
    </div>
    <!--    <input type="text" v-model.trim="modificatorValue">-->
    <!--    <my-button @click="fetchPosts">Получить постыв</my-button>-->
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <post-list v-bind:posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostsLoading"/>
    <div v-else>Идет загрузка...</div>
    <!--    <div class="page__wrapper">-->
    <!--      <div v-for="pageNumber in totalPage"-->
    <!--           :key="page"-->
    <!--           class="page"-->
    <!--           :class="{'current-page': page === pageNumber}"-->
    <!--           @click="changePage(pageNumber)"-->
    <!--           :style="{}"> {{ pageNumber }}</div>-->
    <!--    </div>-->
    <div v-intersection="loadMorePosts" class="observer"></div>
  </div>
</template>

<style>
.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
}

.observer {
  height: 30px;
  background: green;
}
</style>