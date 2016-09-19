vue-scrolllist
==============

Vue 手机端下拉刷新（翻页）组件，页面下拉 -> 触底 -> 请求Api -> 刷新视图。建议es6语法；
当翻页到最后一页时滚动停止请求接口

### 一、安装
cd to you project
```ssh
npm install vue-scrolllist
```
### 二、使用

#### example.vue ####
```html
<template>
  <div>
    <scrolllist :scrollaction="getProducts" :currentpage="currentpage" :pagesize="size" :totalsize="totalElements">
      <div slot="list">
        <div v-for="el in productslist">
            bala bala ...
        </div>
      </div>
    </scrolllist>
  </div>
</template>
```
```js
<script>
// 引入组件
import scrolllist from 'vue-scrolllist';
module.exports = {
  methods: {
    // 翻页方法
    getProducts () {
      // 请务必Promise
      return new Promise((resolve, reject) => {
        ApiRequest.post({
          // 每页条数
          size: this.size, 
          // 当前页
          page: this.currentpage 
        }).then((res) => {
          // success 
          // 接口数据赋值
          this.productslist = [...this.productslist, ...res.data.content];
          // 返回总共条数
          this.totalElements = res.data.totalElements;
          // 每次请求当前＋1，初始化下一页的页码
          this.currentpage = res.data.number + 1;
          // Promise resolve
          resolve();
        }, (res) => {
          // error 
        });
      });
    }
  },
  data () {
    return {
      productslist: [],
      // 定义每页显示多少条
      size: 3,
      // 初始化起始页面，视接口返回第一页的值而定
      currentpage: 0,
      // 总共返回数据，用于计算是否到达结果页
      totalElements: null
    };
  },
  ready () {
    // 首次获取产品
    this.getProducts();
  },
  components: {
    // 初始化组件
    scrolllist
  }
};
</script>
```
