<template>
  <el-menu :default-active="'/index'" router class="el-menu-demo" mode="horizontal" >
    <el-menu-item v-for="(item,i) in navList" :key="i" :index="item.name">{{item.navItem}}</el-menu-item>
    <a href="#nowhere" style="color: #222;float: right;padding: 20px;">更多功能</a>
    <i class="el-icon-menu" style="float:right;font-size: 45px;color: #222;padding-top: 8px"></i>
  </el-menu>
</template>

<script>
export default {
  data() {
    return {
      navList: [
        { name: "/index", navItem: "首页" },
        { name: "/jotter", navItem: "笔记本" },
        { name: "/library", navItem: "图书馆" },
        { name: "/admin", navItem: "个人中心" }
      ]
    };
  }
};
</script>

<style>
</style>