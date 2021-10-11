<template>
  <div class="app-container">
    <Header title="购物车案例"></Header>
    <Goods v-for="item in list" :key="item.id" :id="item.id" :title="item.goods_name" :price="item.goods_price" :img="item.goods_img" :state="item.goods_state" :count="item.goods_count" @state-change="getNewState">
      <Counter :count="item.goods_count" @count-change="getCount(item, $event)"></Counter>
    </Goods>
    <Footer :isfull="fullstate" :amount="amt" :all="total" @full-change="getFullState"></Footer>
  </div>
</template>

<script>
import axios from 'axios'
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
// s 兄弟间传值
// import bus from '@/components/eventBus.js'
// s 插槽
import Counter from '@/components/Counter/Counter.vue'

export default {
  data() {
    return {
      list: []
    }
  },
  created() {
    this.getCartList()
    // s 兄弟间传值
    /* bus.$on('share', val => {
      this.list.some(item => {
        if (item.id === val.id) {
          item.goods_count = val.value
          return true
        }
      })
    }) */
  },
  methods: {
    async getCartList() {
      const { data: res } = await axios.get('https://www.escook.cn/api/cart')
      console.log(res)
      if (res.status === 200) {
        this.list = res.list
      }
    },
    getFullState(val) {
      this.list.forEach(item => {
        item.goods_state = val
      })
    },
    getNewState(e) {
      this.list.some(item => {
        if (item.id === e.id) {
          item.goods_state = e.value
          // 终止后续的循环
          return true
        }
      })
    },
    getCount(item, e) {
      item.goods_count = e
    }
  },
  computed: {
    fullstate() {
      return this.list.every(item => item.goods_state)
    },
    amt() {
      return this.list.filter(item => item.goods_state).reduce((t, item) => (t += item.goods_price * item.goods_count), 0)
    },
    total() {
      return this.list.filter(item => item.goods_state).reduce((t, item) => (t += item.goods_count), 0)
    }
  },
  components: {
    Header,
    Goods,
    Footer,
    // s 插槽
    Counter
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
