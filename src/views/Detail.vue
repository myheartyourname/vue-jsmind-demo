<template>
  <div class="detail">
    <button @click="handleBack">返回上一页</button>
    这是详情页
  </div>
</template>

<script>
export default {
  data () {
    return {}
  },
  methods: {
    handleBack () {
      this.$router.go(-1)
    }
  },
  components: {}
}
</script>

<style>
</style>
