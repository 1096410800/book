<template>
  <div>
    <i class="el-icon-circle-plus-outline" @click="dialogFormVisible = true"></i>
    <el-dialog title="添加/修改图书" :visible.sync="dialogFormVisible">
      <el-form style="text-align: left" v-model="form" ref="dataForm">
        <el-form-item label="书名" :label-width="formLabelWidth" prop="title">
          <el-input v-model="form.title"></el-input>
        </el-form-item>
        <el-form-item label="作者" :label-width="formLabelWidth" prop="author">
          <el-input v-model="form.author"></el-input>
        </el-form-item>
        <el-form-item label="出版日期" :label-width="formLabelWidth" prop="date">
          <el-input v-model="form.date"></el-input>
        </el-form-item>
        <el-form-item label="出版社" :label-width="formLabelWidth" prop="press">
          <el-input v-model="form.press"></el-input>
        </el-form-item>
        <el-form-item label="封面" :label-width="formLabelWidth" prop="cover">
          <el-input  type="hidden" v-model="form.cover" ></el-input>
          <upload @onUpload="upload" ref="file"></upload>
        </el-form-item>
        <el-form-item label="简介" :label-width="formLabelWidth" prop="abs">
          <el-input v-model="form.abs" type="textarea"></el-input>
        </el-form-item>
        <el-form-item label="分类" :label-width="formLabelWidth" prop="title">
          <el-select v-model="form.cid">
            <el-option label="文学" value="1"></el-option>
            <el-option label="流行" value="2"></el-option>
            <el-option label="文化" value="3"></el-option>
            <el-option label="生活" value="4"></el-option>
            <el-option label="经管" value="5"></el-option>
            <el-option label="科技" value="6"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item prop="id" style="height: 0">
          <el-input type="hidden" v-model="form.id" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取消</el-button>
        <el-button type="primary" @click="onSubmit">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>


<script>
import Upload from '@/components/common/Upload'
export default {
  name: "EditForm",
  components:{Upload},
  data() {
    return {
      dialogFormVisible: false,
      formLabelWidth: "120px",
      form: {
        id: "",
        title: "",
        author: "",
        date: "",
        press: "",
        cover: "",
        abs: "",
        cid:""
      }
    };
  },
  methods: {
    onSubmit() {
      this.$axios
        .post("library/book/add", {
          id: this.form.id,
          cover: this.form.cover,
          title: this.form.title,
          author: this.form.author,
          date: this.form.date,
          press: this.form.press,
          abs: this.form.abs,
          cid: this.form.cid
        })
        .then(resp => {
          if (resp && resp.data.status === 0) {
           this.dialogFormVisible = false;
           this.$emit('onSubmit')
          }
        });
    },
    upload(url){
      this.form.cover=url
    }
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