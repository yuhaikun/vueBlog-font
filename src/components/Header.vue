<template>
  <div class="m-content">
    <h3>欢迎来到Rewind的博客</h3>
    <div class="block"><el-avatar :size="50" :src="user.avatar"></el-avatar></div>
    <div>{{ user.username }}</div>

    <div class="maction">
      <span><el-link href="/blogs">主页</el-link></span>
      <el-divider direction="vertical"></el-divider>
      <span><el-link type="success" href="/blog/add">发表文章</el-link></span>

      <el-divider direction="vertical"></el-divider>
      <span v-show="!hasLogin"><el-link type="primary" href="/login">登陆</el-link></span>


      <span v-show="hasLogin"><el-link type="danger" @click="logout">退出</el-link></span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Header.vue",
  data() {
    return {
      user: {
        username: '请先登陆',
        avatar: 'https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png'
      },
      hasLogin: false
    }
  },
  methods: {
    //jwt是无状态的，没有传递给redis进行状态处理，所以只需要清空localstorge就行了
    logout () {
      const _this = this
      _this.$axios.get("/logout",{
        headers: {
          "Authorization": localStorage.getItem("token")
        }
      }).then( res => {
          _this.$store.commit("REMOVE_INFO")
          _this.$router.push("/login")
      })
    }
  },
  created() {
    if(this.$store.getters.getUser.username) {
      this.user.username = this.$store.getters.getUser.username
      this.user.avatar = this.$data.user.avatar

      this.hasLogin = true
    }
  }
}
</script>

<style scoped>

  .m-content {
    max-width: 960px;
    margin: 0 auto;
    text-align: center;
  }
  .maction {
    margin: 10px 0;
  }
</style>