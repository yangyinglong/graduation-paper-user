<template>
  <div class="home-carousel">
    <!-- <el-carousel indicator-position="outside" class="hello">
      <el-carousel-item v-for="item in 4" :key="item">
        <h3>{{ item }}</h3>
      </el-carousel-item>
    </el-carousel> -->
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      msg: 'name'
    }
  },
  created(){
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
.home-carousel {
  height: 530px;
  width: 100%;
}

.hello {
  height: 100%;
  width: 80%;
  margin: 0 auto;
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 18px;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
</style>
