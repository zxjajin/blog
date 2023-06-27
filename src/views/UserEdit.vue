<template lang="">
    <div class="main">
        用户名：<el-input  v-model="user.username" class="input" clearable></el-input>
        头像：<el-input  v-model="user.avatar" class="input" clearable></el-input>
        邮箱：<el-input  v-model="user.email" class="input" clearable></el-input>
        密码：<el-input show-password v-model="user.password" class="input" clearable></el-input>
        <div class="btn" @click="updateUser()"><el-button  type="primary">修改</el-button></div>
        <template>
	<div>
		选择多个文件：<input @change="getFiles($event)" name="files" type="file" multiple="multiple" /><br />
		选择文件夹，遍历该文件夹下所有文件：<input @change="getFiles($event)" name="files" type="file" webkitdirectory mozdirectory /><br />
		<button @click="upload" type="submit">上传</button>
	</div>
</template>

    </div>
    
</template>
<script>
export default {
    data(){
        return{
            user:{
                id:'',
                username:'',
                avatar:'',
                email:'',
                password:''
            },
            files: []
        }
    },
    methods: {
			getFiles: function(event) {
				var files = event.target.files;
				for (var i = 0; i < files.length; i++) {
                    this.files.push(files[i]);
				}
			},
			upload: function() {
				var formData = new FormData();
				for (var i = 0; i < this.files.length; i++) {
					formData.append('files', this.files[i]);
				}
				this.$axios.post("/user/upload", formData, {
					headers: {
						'Content-Type': 'multipart/form-data'
					}
				}).then(function(response) {
					// 请求成功
                    console.log(response)
				}, function(err) {

				})
			},
            updateUser(){
                const user = this.user
                this.$axios( {
                    url:'/user/update',
                    // data:{
                    //     user:this.user
                    //     // id:this.user.id,
                    //     // username:this.user.username,
                    //     // avatar:this.user.avatar,
                    //     // email:this.user.email,
                    //     // password:this.user.password
                    // },
                    data:this.user,
                    method:"PUT",
              headers: {
                "Authorization": localStorage.getItem("token")
                
              }
            }).then(res=>{
                console.log(res)
            })
            // this.$axios.put("/user/update",this.user,{
            //     headers: {
            //     "Authorization": localStorage.getItem("token")
                
            //   }
            // }).then(res=>{
            //     console.log(res)
            // })
            }
		},
    created(e){
        const userid = this.$route.query.id
         this.$axios({
          url:"/user/"+userid,
        }).then(res=>{
          console.log(res)
        //   this.user = res.data.data
          this.user.id = res.data.data.id
          this.user.username = res.data.data.username
          this.user.avatar = res.data.data.avatar
          this.user.email = res.data.data.email
          this.user.password = res.data.data.password
          console.log('user',this.user)
        }).catch(err=>{

        })
    }
}
</script>
<style scoped>
.main{
width: 400px;
margin: 0 auto;
}
.btn{
    margin-top: 20px;
    text-align: center;
}
</style>