<template>
  <div class="hello">
    <button @click="login">登录</button>
    <p>{{msg}}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'name'
    }
  },
  methods: {
    login(){
      this.$http.Login("Bob").then((result) => {
        if (result.c === 200) {
          this.msg = result.r
        } else {
          this.msg = "登录失败"
        }
      }, (err) => {
        this.$message.error(err.msg)
      })
    }
  }

}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
