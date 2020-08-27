<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
  // 加载引入Home组件
  import vHome from './components/common/Home.vue'
  import Header from './components/common/Header'
  import Sidebar from './components/common/Sidebar'

  export default {
    components: {
      // 挂载Home组件
      vHome,
      Header,
      Sidebar
    }
  }
</script>

<style lang="less">
  @import "./assets/css/main.css";
  @import "./assets/css/color-dark.css";

  *{
    margin: 0;
    padding: 0;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
</style>