<template>
  <div class="authorinfo">
    <div class="authorsummay">
      <div class="topbar">作者</div>
      <router-link :to="{ name: 'user_info', params: {name: userinfo.loginname} }">
        <img :src="userinfo.avatar_url" alt="">
      </router-link>
      <router-link :to="{ name: 'user_info', params: {name: userinfo.loginname} }">
        <span class="loginname">{{userinfo.loginname}}</span>
      </router-link>
    </div>
    <div class="recent_topics">
      <div class="topbar">作者最近主题</div>
      <ul>
        <li v-for="list in topiclimited5">
          <router-link :to="{
            name: 'post_content',
            params: {
              id: list.id,
              name: list.author.loginname
            }
          }">
            {{list.title}}
          </router-link>
        </li>
      </ul>
    </div>
    <div class="recent_replies">
      <div class="topbar">作者最近回复</div>
      <ul>
        <li v-for="list in replylimited5">
          <router-link :to="{
            name: 'post_content',
            params: {
              id: list.id,
              name: list.author.loginname
            }
          }">
            {{list.title}}
          </router-link>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: "SlideBar",
    data(){
      return {
        userinfo: {}
      }
    },
    methods: {
      getData() {
        this.$http.get('https://cnodejs.org/api/v1/user/' + this.$route.params.name)
          .then(response => {
            this.isLoading = false
            this.userinfo = response.data.data
          })
          .catch(error => console.log(error))
      }
    },
    computed: {
      topiclimited5(){
        if (this.userinfo.recent_topics) {
          return this.userinfo.recent_topics.slice(0, 5)
        }
      },
      replylimited5() {
        if (this.userinfo.recent_replies) {
          return this.userinfo.recent_replies.slice(0, 5)
        }
      }
    },
    beforeMount(){
      this.isLoading = true
      this.getData()
    },
    watch: {
      '$route'(to, from) {
        this.isLoading = true
        this.getData()
      }
    }
  }
</script>

<style scoped>
  .authorsummay, .recent_replies, .recent_topics {
    background-color: #fff;
    border-radius: 3px;
    overflow: hidden;
  }

  .authorinfo {
    width: 325px;
    float: right;
    margin-top: 0;
  }

  li {
    padding: 3px 0;
  }

  .recent_replies ul, .recent_topics ul {
    margin-top: 0px;
    margin-bottom: 0px;
    list-style: none;
    padding-left: 14px;
  }

  a {
    text-decoration: none;
    color: #778087;
  }

  ul a {
    font-size: 12px;
  }

  .topbar {
    padding: 10px;
    background-color: #f6f6f6;
    line-height: 20px;
    font-size: 12px;
  }

  img {
    height: 48px;
    width: 48px;
    border-radius: 3px;
    margin: 10px;
  }

  .loginname {
    width: 100px;
    float: right;
    margin-top: 25px;
    margin-right: 150px;
    font-size: 14px;
    white-space: nowrap;
  }

  .authorsummay, .recent_topics {
    margin-bottom: 13px;
  }
</style>
