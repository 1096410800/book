<template>
  <div style="margin-bottom: 30px;display: flex;justify-content: center;align-items: center">
    <el-input
      size="small"
      style="width:400px;margin-right: 10px"
      prefix-icon="el-icon-search"
      v-model="keywords"
      placeholder="通过书名或作者搜索..."
    ></el-input>
    <el-button icon="el-icon-search" size="small" type="primary" @click="searchClick">搜索</el-button>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  data() {
    return {
      keywords:'',
      // books:[],
      // cardLoading: []
    }
  },
  methods: {
    searchClick(){
      this.$emit('onSearch')
    }
  },
};
</script>

<style>
</style>