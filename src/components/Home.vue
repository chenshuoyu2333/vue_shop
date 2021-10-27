<template>
  <div>
    <el-button type="info" @click="logout">退出</el-button>
  </div>
</template>

<script>
export default {
  methods: {
    // 退出
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/Login')
    }
  }

}
</script>

<style>

</style>
