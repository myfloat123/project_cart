<template>
  <div>
    <input
      class="btn btn-info"
      type="submit"
      value="Delete"
      @click="deleteById(cartObj.id)"
    >
    <input
      class="btn btn-default"
      type="reset"
      value="SortByName"
      @click="sortByName(cartList)"
    >
  </div>
</template>

<script>
import bus from '../eventBus'
export default {
  name: 'Button',
  props: ['cartList'],
  data() {
    return {
      cartObj: {},
      flag: true
    }
  },
  created() {
    bus.$on('share', ({item}) => {
      this.cartObj = item
      // console.log(this.cartObj);
    })
  },
  methods: {
    deleteById(id) {
      if(id===undefined) {
        alert('请选择您要删除的选项')
      }
      this.$emit('removeId', id)
      this.cartObj = {}

    },
    sortByName(cartList) {
      let cartListByName = []
      if (this.flag) {
        this.flag = false
        cartListByName = cartList.sort((a,b) => a.name.localeCompare(b.name, 'en'))
      } else {
        this.flag = true
        cartListByName = cartList.sort((a,b) => b.name.localeCompare(a.name, 'en'))
      }
      this.$emit('shareNameList', cartListByName)
    }
  }
}
</script>

<style lang="less" scoped>
.btn {
  float: right;
}
.btn.btn-default {
  margin-right: 10px;
  color: #17a2b8;
}
</style>