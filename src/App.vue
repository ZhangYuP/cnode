<template>
  <div id="app">
    <Header></Header>
    <div class="main">
      <router-view name="slidebar"></router-view>
      <router-view name="main"></router-view>
    </div>
  </div>
</template>

<script>
  import Header from './components/Header'
  import PostList from './components/PostList'

  export default {
    name: 'App',
    components: {
      Header,
      PostList
    }
  }
</script>

<style>
  * { margin: 0; padding: 0; box-sizing: border-box;}

  ul, ol, li { list-style: none; }

  .main { width: 90%; margin: 0 auto; min-width: 768px;}

  body { background-color: #e1e1e1; }
</style>
