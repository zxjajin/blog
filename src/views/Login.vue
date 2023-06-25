<template>
  <div class="hello">
    <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick">

      <el-tab-pane label="登录" name="first">
        <div class="div_top2 div">
          <span> 用户名：</span>
          <el-input placeholder="请输入用户名" v-model="username" class="input" clearable></el-input>
        </div>
        <div>
          <span class="m-left">密码：</span>
          <el-input placeholder="请输入密码" @keydown.enter.native="login" v-model="password" class="input" show-password></el-input>
        </div>
        <el-row class="cent">
          <el-button @click="login" type="primary">登录</el-button>
        </el-row>
      </el-tab-pane>
      <el-tab-pane label="注册" name="second">
        <div class="div_top div">
          <span> 用户名：</span>
          <el-input placeholder="请输入用户名" v-model="username" class="input" clearable></el-input>
        </div>
        <div class="div">
          <span class="m-left"> 邮箱：</span>
          <el-input placeholder="请输入邮箱" v-model="email" class="input" clearable></el-input>
        </div>
        <div>
          <span class="m-left">  密码：</span>
          <el-input placeholder="请输入密码" @keydown.enter.native="sign" v-model="password" class="input" show-password></el-input>
        </div>
        <el-row class="cent">
          <el-button @click="sign" type="primary">注册</el-button>
        </el-row>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      activeName: 'first',
      username:'',
      password:'',
      input: '',
      email:'',
      info:[]
    };
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
      // debugger;
    },
    login(){
      this.$axios({
               method:'post',
              url:'/login',
              data:{  
                username:this.username,
                password:this.password
               },
           }).then(res => {  //res是返回结果
               //你的下一步操作 例:
              //  console.log(this)
              //  console.log(res.data)
              const jwt = res.headers['authorization']
              const userInfo = res.data.data
              // console.log("jwt",jwt)
              // console.log("userInfo",userInfo)

              // 把数据共享出去
              this.$store.commit("SET_TOKEN", jwt)
              this.$store.commit("SET_USERINFO", userInfo)
              
              // 获取
              // console.log("获取UserInfo",this.$store.getters.getUser)

              this.$router.push("/blogs")
           }).catch(err => { //请求失败就会捕获报错信息
                 //err.response可拿到服务器返回的报错数据       
                console.log(err)
           })
    },
    sign(){
        this.$axios({
          // url:'http://localhost:8080/sign',
          url:'/user/save',
          method:'post',
          data:{
            username:this.username,
            password:this.password,
            email:this.email,

          }
        }).then(res =>{
          // console.log(res)
          alert("注册成功")
        }).catch(err =>{
          console.log(err)
        })

        
    }
  },
  mounted(){
      
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
span{
  color: white;
}
.m-left{
  margin-left: 15px;
}
.hello {
  width: 510px;
  margin: 100px auto;
  
}
.el-tabs{
  height: 356px;
  background: url('../../public/static/img/c.jpg');
  background-size: contain;
}
.el-tab-pane{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.div {
  
  margin-bottom: 20px;
}
.div_top{
margin-top: 60px;
}
.div_top2{
margin-top: 80px;
}

.input {
  width: 300px;
}
.cent{
  margin-top: 20px;
  text-align: center;
}
/* ::v-deep .el-input__inner{
  background: pink;
  color: pink;
} */

</style>
