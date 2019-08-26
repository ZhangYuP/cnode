<template>
  <div class="PostList">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <div class="posts" v-else>
      <ul>
        <li>
          <div class="topbar">
            <span @click="switchTab('')" :class="{selected: tab === ''}">全部</span>
            <span @click="switchTab('good')" :class="{selected: tab === 'good'}">精华</span>
            <span @click="switchTab('share')" :class="{selected: tab === 'share'}">分享</span>
            <span @click="switchTab('ask')" :class="{selected: tab === 'ask'}">问答</span>
            <span @click="switchTab('job')" :class="{selected: tab === 'job'}">招聘</span>
          </div>
        </li>
        <li v-for="post in posts">
          <img :src="post.author.avatar_url" alt="">
          <span class="allcount">
            <span class="reply_count">{{post.reply_count}}</span>
            /{{post.visit_count}}
          </span>
          <span :class="[{put_good: post.good === true, put_top: post.top === true,
           'topiclist-tab': post.good !== true && post.top !== true}]">
            <span>{{post | tabFormatter}}</span>
          </span>
          <router-link :to="{
            name: 'post_content',
            params: {
              id: post.id,
              name: post.author.loginname
            }
          }">
            <span>{{post.title}}</span>
          </router-link>
          <span class="last_reply">{{post.last_reply_at | formatData}}</span>
        </li>
        <li>
          <Pagination @handleList="renderList"></Pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import Pagination from './Pagination'
  export default {
    name: "PostList",
    data(){
      return {
        isLoading: false,
        posts: [],
        postpage: 1,
        tab: ''
      }
    },
    components: {
      Pagination
    },
    methods: {
      getData(){
        this.$http.get('https://cnodejs.org/api/v1/topics',{
          params: {
            page: this.postpage,
            tab: this.tab,
            limit: 20
          }
        }).then(response => {
          this.isLoading = false
          this.posts = response.data.data
        }).catch(error => console.log(error))
      },
      renderList(value){
        this.postpage = value
        this.getData()
      },
      switchTab(tab){
        this.tab = tab
        this.getData()
      }
    },
    beforeMount(){
      this.isLoading = true
      this.getData()
    }
  }
</script>

<style scoped>
  .PostList {
    background-color: #e1e1e1;
  }
  .posts {
    margin-top: 10px;
    border-radius: 3px;
    overflow: hidden;
  }
  .PostList img{
    width: 30px;
    height: 30px;
    vertical-align: middle;
  }
  ul{
    max-width: 1344px;
    margin: 0 auto;
  }
  li:not(:first-child){
    padding: 9px;
    font-size: 15px;
    font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
    font-weight: 400;
    background-color: white;
    color: #333;
    border-top: 1px solid #f0f0f0;
  }
  li:not(:first-child):hover{
    background: #f5f5f5;
  }
  li span{
    line-height: 30px;
  }
  .allcount{
    width: 70px;
    display: inline-block;
    text-align: center;
    font-size: 12px;
  }
  .reply_count{
    color: #9e78c0;
    font-size: 14px;
  }
  .put_good, .put_top{
    background: #80bd01;
    color: #fff;
    padding: 2px 4px;
    border-radius: 3px;
    font-size: 12px;
    margin-right: 10px;
  }
  .topiclist-tab{
    background: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    font-size: 12px;
    margin-right: 10px;
  }
  .last_reply{
    float: right;
    color: #778087;
    font-size: 12px;
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
  }
  .topbar{
    height: 40px;
    background: #f5f5f5;
  }
  .topbar span{
    font-size: 14px;
    color: #80bd01;
    line-height: 40px;
    margin: 0 10px;
    cursor: pointer;
  }
  .topbar span.selected{
    background: #80bd01;
    color: #fff;
    padding: 2px 4px;
    border-radius: 3px;
    font-size: 14px;
  }
  .topbar span:not(.selected):hover{
    color: #9e78c0;
  }
  .loading{
    text-align: center;
    padding-top: 300px;
  }
</style>
