<template>
  <div>
    <Header></Header>

    <div class="mblog">
      <h2> {{ blog.title }}</h2>
      <el-link icon="el-icon-edit" v-if="ownBlog">
        <router-link :to="{name: 'BlogEdit', params: {blogId: blog.id}}" >
        编辑
        </router-link>
       <div class="right"><a  @click.prevent="dele">删除</a></div>
      </el-link>
     
      <el-divider>  </el-divider>
      <div class="markdown-body" v-html="blog.content">
      </div>
     <div class="footer">
      <span>{{ blog.created | mydata }}</span>
        <span>浏览量：{{blog.frequency}}</span> 
     </div>
    </div>

  </div>
</template>

<script>
  import 'github-markdown-css'
  import Header from "../components/Header";

  export default {
    name: "BlogDetail.vue",
    components: {Header},
    data() {
      return {
        blog: {
          // id: "",
          // title: "",
          // content: ""
        },
        ownBlog: false
      }
    },
    methods:{
      dele(){
        console.log(this.blog.id)

        this.$confirm('是否确认删除？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          const _this = this
          this.$axios({
            method:"DELETE",
            url:"/blog/"+this.blog.id,
            headers: {
                "Authorization": localStorage.getItem("token")
              }
          }).then(res=>{
            // console.log(res.data.code)
            var code = res.data.code
            if(code === 200){
              _this.$alert('删除成功', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                  _this.$router.push("/blogs")
                }
              });
            }else{
              _this.$alert('删除成功', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                  _this.$router.push("/blogs")
                }
              });
          }
          }).catch(err =>{
            _this.$alert('请求失败', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                  _this.$router.push("/blogs")
                }
              });
          console.log(err)
        })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });


       
      },
      frequency(blogId){
      this.$axios.post('/blogs/'+blogId).then(res=>{

      })
    },
   
    },
    created() {
      const blogId = this.$route.params.blogId
      // console.log(blogId)
      // const _this = this
      this.$axios.get('/blog/' + blogId).then(res => {
        this.blog = res.data.data
        // _this.blog.id = blog.id
        // _this.blog.title = blog.title

        var MardownIt = require("markdown-it")
        var md = new MardownIt()

        var result = md.render(this.blog.content)
        this.blog.content = result
        this.ownBlog = (this.blog.userId === this.$store.getters.getUser.id)
      })
      this.frequency(blogId)
    },
    filters: {
    mydata(value) {
      // console.log(value)
      return value.replace("T", " ");
    },
  },
  }
</script>

<style scoped>
.mblog{
  max-width: 960px;
  margin: 0 auto;
}
.el-link {
  display: block;
}
.right{
  float: right;
  margin-right: 20px;
}
.right a:hover{
  text-decoration: none;
}
  .mblog {
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    width: 100%;
    min-height: 700px;
    padding: 20px 15px;
  }
.footer{
  display: flex;
  flex-direction: column;
  align-items: end;
  margin-right: 50px;
  margin-top: 10px;
  }
  .footer span{
    
    margin-top: 5px;
  }
</style>