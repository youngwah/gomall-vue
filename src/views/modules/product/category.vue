<template>
  <el-tree :data="menus" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
</template>

<script>
  export default {
    data () {
      return {
        menus: [],
        defaultProps: {
          children: 'children',
          label: 'name'
        }
      }
    },
    methods: {
      handleNodeClick (data) {
        console.log(data)
      },
      getMenu () {
        this.$http({
          url: this.$http.adornUrl('/product/category/list/tree'),
          method: 'get'
        }).then(data => {
          console.log(data)
          this.menus = data.data.data
        })
      }
    },
    created () {
      this.getMenu()
    }
  }
</script>

<style scoped>

</style>
