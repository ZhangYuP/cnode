<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="pagebtns[0] !== 1" class="pagebtn">···</button>
    <button v-for="btn in pagebtns"
            :class="{currentPage: btn === currentPage, pagebtn: true}"
            @click="changeBtn(btn)">
      {{btn}}
    </button>
    <button class="pagebtn">···</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
  import $ from 'jquery'
  export default {
    name: "Pagination",
    data(){
      return {
        pagebtns: [1, 2, 3, 4, 5],
        currentPage: 1
      }
    },
    methods: {
      changeBtn(page){
        if (typeof page !== 'number') {
          switch (page.target.innerText) {
            case '上一页':
              $('button.currentPage').prev().click()
              break
            case '下一页':
              $('button.currentPage').next().click()
              break
            case '首页':
              this.pagebtns = [1, 2, 3, 4, 5]
              this.changeBtn(1)
              break
            default:
              break
          }
          return
        }
        this.currentPage = page
        if (page === this.pagebtns[4]) {
          this.pagebtns.push(page + 1)
          this.pagebtns.shift()
        } else if (page === this.pagebtns[0] && page > 1) {
          this.pagebtns.pop()
          this.pagebtns.unshift(page - 1)
        }
        this.$emit('handleList', this.currentPage)
      }
    }
  }
</script>

<style scoped>
  .pagination {
    margin-top: 5px;
    margin-bottom: 20px;
    background-color: white;
    padding: 6px 20px;
    border-radius: 5px;
    border: 1px solid #888888;
  }

  .pagination::after{
    content: '';
    display: block;
    clear: both;
  }

  button {
    background-color: #fff;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    cursor: pointer;
    padding: 0 2px;
    margin: 0 4px;
    width: 55px;
    height: 29px;
    float: left;
  }

  .pagebtn {
    width: 40px;
  }

  .currentPage {
    color: white;
    background-color: #1f1b1b;

  }
</style>
