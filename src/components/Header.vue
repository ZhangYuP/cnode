<template>
  <div class="header">
    <div class="container">
      <router-link :to="{name: 'root'}">
        <img src="../assets/cnodejs_light.svg" alt="">
      </router-link>
      <ul>
        <li><a href="#">首页</a></li>
        <li><a href="#">新手入门</a></li>
        <li><a href="#">API</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">注册</a></li>
        <li><a href="#">登录</a></li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Header"
  }
</script>

<style scoped>
  .header{
    background: #444444;
    height: 50px;
    min-width: 768px;
  }
  .container{
    margin: 0 auto;
    width: 90%;
    max-width: 1344px;
  }
  img{
    max-width: 120px;
    margin-top: 8px;
  }
  ul{
    float: right;
    margin: 4px;
  }
  li{
    display: inline-block;
    padding: 10px 15px;
  }
  a{
    text-decoration: none;
    color: #cccccc;
    font-size: 13px;
  }
</style>
