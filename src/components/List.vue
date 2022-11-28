<template>
  <div>
    <table class="table table-bordered table-hover">
      <thead>
        <tr>
          <th>Name</th>
          <th>Quantity</th>
          <th>(Standard)Price(USD)</th>
          <th>Sub-total(USD)</th>
        </tr>
      </thead>
      <tbody
        tabindex="0"
        ref="tbodyEl"
      >
        <tr
          v-for="item in cartList"
          :key="item.id"
          @click="handler($event, item)"
          ref="tr"
        >
          <td>{{ item.name }}</td>
          <td @change="handlerQty($event,item)"><input
              type="text"
              v-model.trim="item.qty"
              @click.stop="handlerInput($event)"
              @blur="handlerBlurInput()"
              ref="ipt"
            ></td>
          <td>{{ item.price }}</td>
          <td>{{ item.qty * item.price }}</td>
        </tr>
        <tr>
          <td colspan="3">Total</td>
          <td>USD {{ subTotal }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import bus from '../eventBus'
export default {
  name: 'List',
  props: ['cartList'],
  computed: {
    subTotal() {
      return this.cartList.reduce((amt, item) => (amt += item.qty * item.price), 0)
    }
  },
  methods: {
    handler(e, item) {
      const el = e.target.parentNode
      // console.log(this.$refs.tr,e.target)
      this.$refs.tr.forEach(itemTr => (itemTr.style.backgroundColor = ''))
      el.style.backgroundColor = '#dee2e6'
      bus.$emit('share', { el, item })
      // console.log(this.$refs)
      const domEl = this.$refs // 将this.$refs保存在一个变量中，方便在下面的失去焦点事件中使用
      // this.$refs.tbodyEl.onfocus = function (e) {
      //   console.log('获取焦点',domEl)
      // }
      this.$refs.tbodyEl.onblur = function () {
        // console.log(this); 这里的this不是组件实例，是触发失去焦点事件的dom元素
        // console.log(domEl);
        domEl.tr.forEach(itemTr => (itemTr.style.backgroundColor = ''))
      }
    },
    handlerQty(e, item) {
      let elV = e.target.value
      if (elV < 1 || isNaN(elV)) {
        alert('请输入非零整数')
      } else if (String(elV).indexOf('.') > -1) {
        // console.log(String(elV).indexOf('.'));
        alert('请输入非零整数!')
      } else {
        item.value = elV
      }
    },
    handlerInput(e) {
      e.target.parentNode.style.backgroundColor = ''
      this.$refs.tr.forEach(itemTr => (itemTr.style.backgroundColor = ''))
      e.target.parentNode.parentNode.style.backgroundColor = '#dee2e6'
      e.target.parentNode.click()
      // console.log(e.target.parentNode);
      // console.log(e.target.parentNode.parentNode);
    },
    handlerBlurInput() {
      this.$refs.tr.forEach(itemTr => (itemTr.style.backgroundColor = ''))
    }
  }
}
</script>

<style lang="less" scoped>
.table.table-bordered {
  text-align: right;
  margin-top: 10px;
  margin-bottom: 10px;
  thead {
    tr {
      th:first-child {
        text-align: left;
      }
      th:nth-child(2) {
        text-align: center;
      }
    }
  }
  tbody {
    tr {
      td:first-child {
        text-align: left;
      }
      td:nth-child(2) {
        text-align: center;
        padding: 0;
        input {
          display: flex;
          text-align: center;
          vertical-align: middle;
          width: 100%;
          height: 48px;
          outline: none;
          border: 0;
          background-color: transparent;
        }
      }
    }
    tr:last-child {
      td {
        text-align: right;
      }
    }
  }
}
</style>