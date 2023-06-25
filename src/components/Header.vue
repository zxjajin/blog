<template>
  <div class="m-content">
    <!-- <h3>欢迎来到阿金的博客</h3> -->
    <div class="right">
      <a class="Header-link" href="https://github.com/zxjajin" data-hotkey="g d" aria-label="Homepage " data-turbo="false"
        data-analytics-event='{"category":"Header","action":"go to dashboard","label":"icon:logo"}'>
        <svg height="32" aria-hidden="true" viewBox="0 0 16 16" version="1.1" width="32" data-view-component="true"
          class="octicon octicon-mark-github v-align-middle">
          <path
            d="M8 0c4.42 0 8 3.58 8 8a8.013 8.013 0 0 1-5.45 7.59c-.4.08-.55-.17-.55-.38 0-.27.01-1.13.01-2.2 0-.75-.25-1.23-.54-1.48 1.78-.2 3.65-.88 3.65-3.95 0-.88-.31-1.59-.82-2.15.08-.2.36-1.02-.08-2.12 0 0-.67-.22-2.2.82-.64-.18-1.32-.27-2-.27-.68 0-1.36.09-2 .27-1.53-1.03-2.2-.82-2.2-.82-.44 1.1-.16 1.92-.08 2.12-.51.56-.82 1.28-.82 2.15 0 3.06 1.86 3.75 3.64 3.95-.23.2-.44.55-.51 1.07-.46.21-1.61.55-2.33-.66-.15-.24-.6-.83-1.23-.82-.67.01-.27.38.01.53.34.19.73.9.82 1.13.16.45.68 1.31 2.69.94 0 .67.01 1.3.01 1.49 0 .21-.15.45-.55.38A7.995 7.995 0 0 1 0 8c0-4.42 3.58-8 8-8Z">
          </path>
        </svg>
      </a>
    </div>
    <div class="block">
      <el-avatar :size="50" :src="user.avatar"></el-avatar>
      <!-- <img src="../assets/l" alt=""> -->
      <div>{{ user.username }}</div>
    </div>

    <div class="maction">
      <span><el-link href="/blogs">主页</el-link></span>
      <el-divider direction="vertical"></el-divider>
      <span><el-link type="success" href="/blog/add">发表博客</el-link></span>

      <el-divider direction="vertical"></el-divider>
      <span v-show="!hasLogin"><el-link type="primary" href="/login">登录</el-link></span>
      <span v-show="hasLogin"><el-link type="danger" @click="logout">退出</el-link></span>
      <el-divider direction="vertical"></el-divider>
      <span><el-link href="http://8.138.58.49:3000/forecast" type="blue" target="blank">天气</el-link></span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      flag: false,
      input: "",
      user: {
        username: "请先登录",
        avatar:
          "http://8.138.58.49:9999/a.png",
      },
      hasLogin: false,
    };
  },
  methods: {
    logout() {
      const _this = this;
      _this.$axios
        .get("/logout", {
          headers: {
            Authorization: localStorage.getItem("token"),
          },
        })
        .then((res) => {
          _this.$store.commit("REMOVE_INFO");
          _this.$router.push("/login");
        });
    },
  },
  created() {
    // console.log("获取UserInfo", this.$store.getters.getUser);
    if (this.$store.getters.getUser) {
      this.user.username = this.$store.getters.getUser.username;
      this.user.avatar = this.$store.getters.getUser.avatar;
      console.log("头像", this.user.avatar);

      this.hasLogin = true;
    }
  },
};
</script>

<style scoped>
.m-content {
  position: relative;
  padding-top: 20px;
}

.maction span {
  margin: 0 10px;
}

.right {
  position: absolute;
  top: 98px;
  right: 290px;
}

.m-content {
  max-width: 960px;
  margin: 0 auto;
  text-align: center;
}

.maction {
  margin: 10px 0;
}
</style>