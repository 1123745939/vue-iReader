<template>
  <transition name="slide-down">
    <div class="title-wrapper"
      v-show="TitleAndMenuShow">
      <div class="left">
        <span class="icon icon-back"></span>
      </div>
      <div class="right">
        <div class="icon-wrapper">
          <span class="icon icon-cart"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon icon-person"></span>
        </div>
        <div class="icon-wrapper">
          <span class="icon icon-more"></span>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/javascript">
export default {
  props: {
    TitleAndMenuShow: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/styles/global.scss';
.title-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 101;
  display: flex;
  width: 100%;
  height: px2rem(48);
  background-color: #fff;
  box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);
  .left {
    flex: 0 0 px2rem(60);
    @include center;
  }
  .right {
    flex: 1;
    display: flex;
    justify-content: flex-end;
    .icon-wrapper {
      flex: 0 0 px2rem(40);
      @include center;
    }
  }
}
</style>
