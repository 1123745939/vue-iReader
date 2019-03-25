<template>
  <transition name="slide-right">
    <div class="content">
      <div class="content-wrapper" v-if="bookAvailabe">
        <div class="content-item"
          v-for="(item, index) in navigation.toc"
          :key="index"
          @click="jumpTo(item.href)">
          <span class="text">{{ item.label }}</span>
        </div>
      </div>
      <div class="empty" v-else><span>加载中...</span></div>
    </div>
  </transition>
</template>

<script type="text/javascript">
export default {
  props: {
    bookAvailabe: Boolean,
    navigation: Object
  },
  methods: {
    jumpTo(target) {
      this.$emit('jumpTo', target)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/global.scss';
.content {
  position: absolute;
  top: 0;
  left: 0;
  width: 70%;
  height: 100%;
  background-color: #fff;
  z-index: 102;
  .content-wrapper {
    width: 100%;
    height: 100%;
    overflow:scroll;
    .content-item {
      font-size: px2rem(18);
      height: px2rem(40);
      line-height: px2rem(40);
      padding-left: px2rem(20);
      &:nth-child(n+1) {
        border-bottom: px2rem(1) solid #ddd;
      }
    }
  }
  .empty {
    display: flex;
    height: 100%;
    width: 100%;
    @include center;
  }
}
</style>
