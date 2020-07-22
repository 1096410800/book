<template>
  <div>
    <i class="el-icon-circle-plus-outline" @click="dialogFormVisible = ture"></i>
    <el-dialog title="添加/修改图书" :visible.sync="dialogFormVisible"></el-dialog>
  </div>
</template>

<script>
export default {
  name: "EditForm",
  data() {
    return {
      dialogFormVisible: false,
    };
  }
};
</script>

<style>
.el-icon-circle-plus-outline {
  margin: 50px 0 0 20px;
  font-size: 100px;
  float: left;
  cursor: pointer;
}
</style>