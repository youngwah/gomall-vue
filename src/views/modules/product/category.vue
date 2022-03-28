<template>
  <el-tree :data="menus" :props="defaultProps" :expand-on-click-node="false" show-checkbox node-key="catId"
        :default-expanded-keys="expandedKey">
       <span class="custom-tree-node" slot-scope="{ node, data }">
      <span>{{ node.label }}</span>
      <span>
        <el-button
          v-if="node.level <= 2"
          type="text"
          size="mini"
          @click="() => append(data)"
        >
          Append
        </el-button>
        <el-button
          v-if="node.childNodes.length==0"
          type="text"
          size="mini"
          @click="() => remove(node, data)"
        >
          Delete
        </el-button>
      </span>
    </span>
  </el-tree>
</template>

<script>
  export default {
    data () {
      return {
        menus: [],
        expandedKey: [],
        defaultProps: {
          children: 'children',
          label: 'name'
        }
      }
    },
    methods: {
      append  (data) {
        console.log(data)
      },
      remove (node, data) {
        console.log(node.parent.data.catId)
        var ids = [data.catId]

        this.$confirm(`是否删除【${data.name}】当前菜单?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
          .then(() => {
            this.$http({
              url: this.$http.adornUrl('/product/category/delete'),
              method: 'post',
              data: this.$http.adornData(ids, false)
            })
              .then(({ data }) => {
                this.$message({
                  type: 'success',
                  message: '菜单删除成功!'
                })
              // 刷新出新的菜单
                this.getMenu()
                this.expandedKey = [node.parent.data.catId]
              })
              .catch(() => {})
          })
          .catch(() => {
            this.$message({
              type: 'info',
              message: '已取消删除'
            })
          })
      },
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
