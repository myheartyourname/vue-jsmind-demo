<template>
  <div class="list">
    <div
      class="item"
      v-for="(item, index) in list"
      :key="index"
      @click="handleToDetail(item)"
    >
      {{ item.title }}
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      list: []
    }
  },
  activated () {
    console.log('ssss')
    this.list = [
      { id: '1', title: '1' },
      { id: '2', title: '2' },
      { id: '3', title: '3' },
      { id: '4', title: '4' },
      { id: '5', title: '5' }
    ]
  },
  // mounted () {
  //   console.log('mmmm')
  //   this.list = [
  //     { id: '1', title: '1' },
  //     { id: '2', title: '2' },
  //     { id: '3', title: '3' },
  //     { id: '4', title: '4' },
  //     { id: '5', title: '5' }
  //   ]
  // },
  methods: {
    handleToDetail (item) {
      this.$router.push(`detail/${item.id}`)
    }
  },
  components: {}
}
</script>

<style lang="scss">
.item {
  height: 200px;
  text-align: center;
  border-bottom: 1px solid #eeeeee;
  &.active {
    background-color: #5e5;
  }
}
</style>
