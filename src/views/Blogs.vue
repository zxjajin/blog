<template>
  <div class="main">
    <div class="aside">
      <ul class="ul">
        <li v-for="item in classify" :key="item.id">
          <span @click="getclassity(item.id)"> {{ item.title }}</span>
        </li>
      </ul>
    </div>
    <div class="content">
      <div class="header">
        
        <Header></Header>

      </div>

      <div class="section">
        <div class="relative_div">
          <div class="search">
            <div class="search_input">
              <el-input placeholder="请输入内容" @keydown.enter.native="search()" v-model="input" clearable>
              </el-input>
              <el-button @click="search()" type="success" icon="el-icon-search"></el-button>
            </div>
          </div>
        </div>

        <div class="block">
          <el-timeline>
            <el-timeline-item :timestamp="blog.created | mydata" placement="top" v-for="blog in blogs" :key="blog.id">
              <el-card>
                <h4>
                  <router-link :to="{ name: 'BlogDetail', params: { blogId: blog.id } }">
                    {{ blog.title }}
                  </router-link>
                </h4>
                <p>{{ blog.description }}</p>
                <p>浏览量：{{ blog.frequency }}</p>
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
import Header from "../components/Headerblog.vue";

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
      classify: [],
      flag: false,
      classify_id: "",
    };
  },
  methods: {
    page(currentPage) {
      // this.flag 如果为true就是搜索或者分类查询点击下一页
      if (this.flag) {
        // 这里进一步判断是搜索还是分类下点击下一页
        if (this.input !== "") {
          this.search(currentPage);
        } else if (this.classify_id != "") {
          this.getclassity(this.classify_id, currentPage);
        }
      } else {
        this.$axios({
          url: "/blogs",
          params: {
            currentPage,
          },
        }).then((res) => {
          console.log(res);
          this.blogs = res.data.data.records;
          this.currentPage = res.data.data.current;
          this.total = res.data.data.total;
          this.pageSize = res.data.data.size;
          this.flag = false;
          this.input = "";
          this.classify_id = "";
        });
      }
    },
    search() {
      // console.log(currentPage)

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
        this.flag = true;
        this.classify_id = "";
      });
    },
    search(currentPage) {
      // console.log(currentPage)

      this.$axios({
        method: "GET",
        url: "/blogs/search",
        params: {
          search: this.input,
          currentPage,
        },
      }).then((res) => {
        // console.log(res)
        this.blogs = res.data.data.records;
        this.currentPage = res.data.data.current;
        this.total = res.data.data.total;
        this.pageSize = res.data.data.size;
        this.flag = true;
        this.classify_id = "";
      });
    },
    getTitle() {
      this.$axios("/classify")
        .then((res) => {
          this.classify = res.data.data;
          console.log("分类", this.classify);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getclassity(id, currentPage) {
      // console.log(id)
      this.classify_id = id;
      this.$axios({
        url: "/classify/" + id,
        params: {
          currentPage,
        },
      }).then((res) => {
        console.log(res);
        this.blogs = res.data.data.records;
        this.currentPage = res.data.data.current;
        this.total = res.data.data.total;
        this.pageSize = res.data.data.size;
        this.flag = true;
        this.input = "";
      });
    },
  },
  created() {
    this.page(1);
    this.getTitle();
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
.right {
  position: absolute;
  top: 100px;
    right: 533px;
}


.main {
  display: flex;
  /* position: relative; */
  justify-content: space-between;
  margin: 0px 50px;
  background: url("../../public/static/img/c.jpg");
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
.header{
  position: relative;
}

.header::v-deep .maction {
  /* margin-left: 125px; */
  margin-bottom: 20px;
}

.maction span {
  /* margin: 0 20px; */
}

::v-deep .el-timeline-item__timestamp {
  color: white;
  font-size: 15px;
  /* font-weight: 600; */
}



.m-content {
  max-width: 960px;
  margin: 0 auto;
  text-align: center;
}

.maction {
  margin: 10px 0;
}

.ul {
  display: flex;
  width: 100%;
  margin-top: 180px;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: aliceblue;
}

.ul li {
  margin: 20px;
}


</style>