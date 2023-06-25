<template>
  <div class="main">
    <div class="aside">
      <!-- <Header></Header> -->
      <ul class="ul">
        <li>高数</li>
        <li>JAVA</li>
        <li>C语言</li>
        <li>Vue</li>
        <li>日记</li>
      </ul>
    </div>
    <div class="content">
      <div class="header">
        <!-- <div class="maction">
          <span><el-link href="/blogs">主页</el-link></span>
          <el-divider direction="vertical"></el-divider>
          <span><el-link type="success" href="/blog/add">发表博客</el-link></span>

          <el-divider direction="vertical"></el-divider>
          <span v-show="!hasLogin"><el-link type="primary" href="/login">登录</el-link></span>
          <span v-show="hasLogin"><el-link type="danger" @click="logout">退出</el-link></span>
          <el-divider direction="vertical"></el-divider>
          <span><el-link href="http://8.138.58.49:3000/forecast" type="blue" target="blank">天气</el-link></span>
        </div> -->
        <Header></Header>
      </div>
   
      <div class="section">
      <div class="relative_div">
        <div class="search">
          <div class="search_input">
            <el-input placeholder="请输入内容" @keydown.enter.native="search" v-model="input" clearable>
            </el-input>
            <el-button @click="search" type="success" icon="el-icon-search"></el-button>
          </div>
        </div>
      </div>

      <div class="block">
        <el-timeline>
          <el-timeline-item :timestamp="blog.created | mydata" placement="top" v-for="blog in blogs">
            <el-card>
              <h4>
                <router-link :to="{ name: 'BlogDetail', params: { blogId: blog.id } }">
                  {{ blog.title }}
                </router-link>
              </h4>
              <p>{{ blog.description }}</p>
            </el-card>
          </el-timeline-item>
        </el-timeline>

        <el-pagination class="mpage" background layout="prev, pager, next" :current-page="currentPage"
          :page-size="pageSize" :total="total" @current-change="page">
        </el-pagination>
      </div>
    </div>
    </div>

  </div>
</template>

<script>
import Header from "../components/Header";

export default {
  name: "Blogs.vue",
  components: { Header },
  data() {
    return {
      input: "",
      blogs: {},
      currentPage: 1,
      total: 0,
      pageSize: 5,
    };
  },
  methods: {
    page(currentPage) {
      const _this = this;
      _this.$axios.get("/blogs?currentPage=" + currentPage).then((res) => {
        // console.log(res)
        _this.blogs = res.data.data.records;
        _this.currentPage = res.data.data.current;
        _this.total = res.data.data.total;
        _this.pageSize = res.data.data.size;
      });
    },
    search() {
      this.$axios({
        method: "GET",
        url: "/blogs/search",
        params: {
          search: this.input,
        },
      }).then((res) => {
        // console.log(res)
        this.blogs = res.data.data.records;
        this.currentPage = res.data.data.current;
        this.total = res.data.data.total;
        this.pageSize = res.data.data.size;
      });
    },
  },
  created() {
    this.page(1);
  },
  filters: {
    mydata(value) {
      // console.log(value)
      return value.replace("T", " ");
    },
  },
};
</script>

<style scoped>


.main {
  display: flex;
  /* position: relative; */
  justify-content: space-between;
  margin: 0px 50px;
  background: url('../../public/static/img/c.jpg');
  background-size: contain;
}

.aside {
  width: 300px;
  /* background: pink; */
  margin-right: 6%;
}

.content {
  flex: 1;
  display: flex;
  flex-direction: column;
  /* margin-top: 20px; */
  margin-right: 5%;
}

.relative_div {
  position: relative;
}

.search {
  position: absolute;
  top: -125px;
  right: 30px;
}

.search_input {
  position: relative;
}

.search_input .el-input {
  width: 200px;
  position: absolute;
  top: 0;
  right: 60px;
}

.search_input input {
  position: absolute;
  top: 0;
  right: 0;
}

.mpage {
  margin: 0 auto;
  text-align: center;
  margin-bottom: 20px;
}

.m-content {
  position: relative;
}

.header::v-deep .maction {
  /* margin-left: 125px; */
  margin-bottom: 20px;
}

.maction span {
  /* margin: 0 20px; */
}

.right {
  position: absolute;
  top: 76px;
  right: -40px;
}

.m-content {
  max-width: 960px;
  margin: 0 auto;
  text-align: center;
}

.maction {
  margin: 10px 0;
}
.ul{
  display: flex;
  width: 100%;
  height: 600px;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.ul li{
  margin: 20px;
}
</style>