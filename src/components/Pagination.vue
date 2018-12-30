<template>
  <div class="pagination">
    <!-- @click 不传递参数会默认传递原生的[Mouse Event] -->
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="jduge" class="pagebtn">...</button>
    <!-- 第一个 currentPage 是类名，第二个 currentPage 是 data 中要渲染的那个 -->
    <button v-for="btn in pagebtns" @click="changeBtn(btn)"
    :class="[{currentPage:btn === currentPage},'pagebtn']">
      {{btn}}
    </button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
  import $ from 'jquery'

  export default {
    name: "Pagination",
    data() {
      return {
        pagebtns: [1,2,3,4,5,'...'], // 此数组存的是上一页下一页之间显示的数组，这里是默认的初始值
        currentPage: 1, // 当前选中的页码
        jduge: false
      }
    },
    methods: {
      // 方法是有参数的，上面 @click 绑定处也一定要有参数传入
      changeBtn(page) {
        // 点击上一页、下一页、首页
        if(typeof page !== 'number') {
          // page.target.innerText 表示点击的那个按钮
          switch(page.target.innerText) {
            case '上一页': 
              $('button.currentPage').prev().click();
              break;
            case '下一页': 
              $('button.currentPage').next().click();
              break;
            case '首页': 
              this.pagebtns = [1,2,3,4,5,'...'];
              this.changeBtn(1);
              break;
            default:
              break;
          }
          return;
        }
        this.currentPage = page;
        if(page > 4) {
          this.jduge = true;
        }else {
          this.jduge = false;
        }
        if(page === this.pagebtns[4]) {
          this.pagebtns.shift(); // 移除第一个元素
          this.pagebtns.splice(4,0,this.pagebtns[3]+1); // 添加最后一个
        }else if(page === this.pagebtns[0] && page != 1) {
          // 在第一个位置增加一个
          this.pagebtns.unshift(this.pagebtns[0]-1);
          // 移除最后一个数字
          this.pagebtns.splice(5,1);
        }
        this.$emit('handleList',this.currentPage);
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
    /*box-shadow: 0px 2px 9px #888888;*/
    border: 1px solid #888888;
  }

  button {
    background-color: #fff;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    height: 21px;
    cursor: pointer;
    padding: 0 2px;
    width: 55px;
    height: 29px;
  }

  .pagebtn {
    position: relative;
    bottom: 1px;
    width: 40px;
    margin: 0 4px;
  }

  .currentPage {
    color: white;
    background-color: #1f1b1b;

  }
</style>