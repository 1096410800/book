<template>
  <div>
    <el-row style="height:840px">
      <search-bar ref="searchBar" @onSearch="searchResult"></search-bar>
      <el-tooltip
        effect="dark"
        placement="right"
        v-for="item in books.slice((currentPage-1)*pagesize,currentPage*pagesize)"
        :key="item.id"
      >
        <p slot="content" style="font-size:14px;margin-bottom:6px">{{item.title}}</p>
        <p slot="content" style="font-size:13px;margin-bottom:6px">
          <span>{{item.author}}</span>/
          <span>{{item.date}}</span>/
          <span>{{item.press}}</span>
        </p>
        <p slot="content" style="width:300px" class="abstract">{{item.abs}}</p>
        <el-card
          style="width: 135px;margin-bottom: 20px;height: 233px;float: left;margin-right: 15px"
          class="book"
          bodyStyle="padding:10px"
          shadow="hover"
        >
          <div class="cover" @click="editBook(item)">
            <img :src="item.cover" alt="封面" />
          </div>
          <div class="info">
            <div class="title">
              <a href>{{item.title}}</a>
            </div>
            <i class="el-icon-delete" @click="deleteBook(item.id)"></i>
          </div>
          <div class="author">{{item.author}}</div>
        </el-card>
      </el-tooltip>
      <edit-form @onSubmit="loadBooks()" ref="edit"></edit-form>
    </el-row>
    <el-row>
      <el-pagination
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-size="pagesize"
        :total="books.length"
      ></el-pagination>
    </el-row>
  </div>
</template>

<script>
import EditForm from "./EditForm";
import SearchBar from "./SearchBar";
export default {
  name: "Books",
  components: { EditForm, SearchBar },
  data() {
    return {
      books: [],
      currentPage: 1,
      pagesize: 17
    };
  },
  created() {
    this.loadBooks();
  },
  methods: {
    loadBooks() {
      var _this = this;
      this.$axios.post("library/book/list", {}).then(resp => {
        if (resp && resp.data.status === 0) {
          _this.books = resp.data.message;
        }
      });
    },
    handleCurrentChange(currentPage) {
      this.currentPage = currentPage;
    },
    searchResult() {
      var _this = this;
      this.$axios
        .post("library/book/name/like", {
          nameOrAuthor: this.$refs.searchBar.keywords
        })
        .then(resp => {
          if (resp && resp.data.status === 0) {
            _this.books = resp.data.message;
          }
        });
    },
    editBook(item) {
      this.$refs.edit.dialogFormVisible = true;
      this.$refs.edit.form = {
        id: item.id,
        cover: item.cover,
        title: item.title,
        author: item.author,
        date: item.date,
        press: item.press,
        abs: item.abs,
        cid: item.cid
      };
    },
    deleteBook(id){
      this.$confirm("删除后不可恢复",{
        confirmButtonText:'取消',
        confirmButtonText:'确定',
        type:'warning'
      }).then(()=>{this.$axios.post('library/book/remove',{id:id}).then(resp=>{
        if(resp && resp.data.status === 0){
          this.loadBooks()
        }
      })})
    }
  }
};
</script>

<style>
.cover {
  width: 115px;
  height: 172px;
  margin-bottom: 7px;
  overflow: hidden;
  cursor: pointer;
}

img {
  width: 115px;
  height: 172px;
  /*margin: 0 auto;*/
}

.title {
  font-size: 14px;
  text-align: left;
}

.author {
  color: #333;
  width: 102px;
  font-size: 13px;
  margin-bottom: 6px;
  text-align: left;
}

.abstract {
  display: block;
  line-height: 17px;
}

a {
  text-decoration: none;
}
.el-icon-delete {
  cursor: pointer;
  float: right;
}

a:link,
a:visited,
a:focus {
  color: #3377aa;
}
</style>