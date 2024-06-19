<script>
import Main from "@/pages/Main.vue";
import Navbar from "@/components/UI/Navbar.vue";

export default {
  components: {Navbar, Main}

}
</script>


<template>
  <navbar></navbar>
  <div class="app">
    <router-view></router-view>
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

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>