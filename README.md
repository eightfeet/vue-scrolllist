vue-scrolllist
==============

Vue 手机端下拉刷新（翻页）组件，页面下拉 -> 触底 -> 请求Api -> 刷新视图。建议es6语法；

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
import scrolllist from 'vue-scrolllist';

module.exports = {
  methods: {
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
      size: 3,
      currentpage: 0,
      totalElements: null
    };
  },
  ready () {
    // 首次获取产品
    this.getProducts();
  },
  components: {
    scrolllist
  }
};
</script>
```
