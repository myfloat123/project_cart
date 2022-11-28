<template>
  <div class="container">
    <b>Cart</b>
    <List :cartList="cartList"/>
    <Button @removeId="removeId" :cartList="cartList" @shareNameList="getNameList" />
  </div>
</template>

<script>
import axios from 'axios'
import bus from './eventBus'
import List from '@/components/List.vue'
import Button from '@/components/Button.vue'
export default {
  data() {
    return {
      cartList: [],
    }
  },
  created() {
    this.getCartList()
  },
  methods: {
    async getCartList() {
      const {data} = await axios.get('/cart.json') 
      this.cartList = data
      // console.log(data)
    },
    removeId(id) {
      // console.log(id);
      this.cartList = this.cartList.filter(item => item.id !== id)
    },
    getNameList(val) {
      this.cartList = val
    }
  },
  components: {
    List,
    Button
  }
}
</script>

<style lang="less" scoped>
.container {
  margin: 20px auto;
}
</style>