<template>

  <router-view/>
</template>


<script setup lang="js">
import {provide} from "vue"
import * as echarts from "echarts"
import * as axios from "axios"
//import * as dataview from '@jiaminghi/data-view'
provide("echarts",echarts)
provide("axios",axios)
//provide("dataview",dataview)
</script>







<style lang="less">
// body{
//   background: url("~@/assets/bj.jpg") top center no-repeat;
// }
// *{
//   margin: 0px;
//   padding: 0px;
//   box-sizing: border-box;
// }
html {
  height: 100%; // 让html占满整个视口高度
}
body {
  margin: 0;
  padding: 0;
  height: 100%; // 让body高度等于html（即视口高度）
  // 背景图核心配置：添加cover实现全屏填充
  background: url("~@/assets/4.png") top center no-repeat;
  background-color: black;
  background-size: cover; // 关键：让背景图等比例缩放覆盖整个body
  background-attachment: fixed; // 可选：背景图固定，滚动页面不位移
  overflow-x: hidden; // 可选：防止横向滚动
}
* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}
</style>
