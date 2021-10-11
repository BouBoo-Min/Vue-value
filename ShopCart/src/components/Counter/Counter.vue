<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ count }}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
// s 兄弟间传值
// import bus from '@/components/eventBus.js'

export default {
  props: {
    count: {
      type: Number,
      default: 1
    }
    // s 兄弟间传值
    /* id: {
      type: Number,
      required: true
    } */
  },
  methods: {
    // s 兄弟间传值
    /* add() {
      bus.$emit('share', { id: this.id, value: this.count + 1 })
    },
    sub() {
      if (this.count - 1 === 0) return
      bus.$emit('share', { id: this.id, value: this.count - 1 })
    } */
    add() {
      this.$emit('count-change', this.count + 1)
    },
    sub() {
      if (this.count - 1 === 0) return
      this.$emit('count-change', this.count - 1)
    }
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
