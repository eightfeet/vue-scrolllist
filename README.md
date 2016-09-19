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

### 三、参数说明
<table>
	<tr>
		<th>参数</th>
		<th>介绍</th>
		<th>类型</th>
		<th>备注</th>
	</tr>
	<tr>
    		<td>scrollaction</td>
		<td>请求接口，翻页方法</td>
		<td>function</td>
		<td>必填,请务必 return Promise 对象</td>
  	</tr>
  	<tr>
    		<td>currentpage</td>
		<td>当前页面</td>
		<td>string</td>
		<td>必填,提供当前页面值给组件计算</td>
  	</tr>
  	<tr>
    		<td>pagesize</td>
		<td>每页显示条数</td>
		<td>string</td>
		<td>必填,提供当前页面值给组件计算</td>
  	</tr>
	<tr>
    		<td>totalsize</td>
		<td>接口数据条数</td>
		<td>string</td>
		<td>必填,提供当前页面值给组件计算</td>
  	</tr>
  	<tr>
    		<td>scrollname</td>
		<td>滚动列表名称</td>
		<td>string</td>
		<td>选填,如果一个页面中有多个滚动条时要通过给组建命名加以区分</td>
  	</tr>
</table>

### PS：
父级dom位置决定组件位置
![](./ex.png)
