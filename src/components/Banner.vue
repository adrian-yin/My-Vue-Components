<template>
  <div
    class="banner-wrapper"
    :style="{height:`${bannerHeight}rem`,width:`${bannerWidth}rem`}"
  >
    <div
      class="items-container"
      :style="{width:`${containerWidth}rem`,transition:slideTransition,transform:slideTransform}"
    >
      <div
        class="banner-item"
        v-for="item in items"
        :key="item.id"
        @click="handleItemClick(item)"
        @keyDown="handleItemClick(item)"
      >
        <img
          :src="item.imgUrl"
          :alt="item.imgAlt"
          :style="{height:`${bannerHeight}rem`,width:`${bannerWidth}rem`}"
        />
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'BannerComponent',
  props: {
    // banner宽高（图片宽高），单位rem
    bannerHeight: {
      type: Number,
      default: 5,
    },
    bannerWidth: {
      type: Number,
      default: 10,
    },
    // 滑动动画时间，单位ms
    slideTime: {
      type: Number,
      default: 1000,
    },
    // 滑动动画模式 linear|ease|ease-in|ease-out|ease-in-out
    slideMode: {
      type: String,
      default: 'linear',
    },
    // 每个图片停留时间，单位ms
    stayTime: {
      type: Number,
      default: 4000,
    },
    // 图片对象{id: Numer从1开始, imgUrl: String, imgAlt: String, toUrl: String}
    picItems: Array,
  },
  computed: {
    containerWidth() {
      return this.bannerWidth * (this.items.length + 1);
    },
    slideTransition() {
      // slideTime毫秒时间换算
      const transitionTimeString = (this.transitionTime / 1000).toString();
      return `${transitionTimeString}s ${this.slideMode}`;
    },
    slideTransform() {
      return `translateX(-${this.bannerWidth * (this.currentItemId - 1)}rem)`;
    },
  },
  mounted() {
    // 将第一张图片添加到最后，保证最后的平滑切换
    this.repeatFirstItem();
    this.slideStart();
  },
  beforeDestroy() {
    this.slideStop();
  },
  data() {
    return {
      currentItemId: 1,
      slideInterval: null,
      transitionTime: this.slideTime,
      items: this.picItems,
    };
  },
  methods: {
    repeatFirstItem() {
      // 不超过一张图时无需添加
      if (this.items.length <= 1) {
        return;
      }
      const firstItem = JSON.parse(JSON.stringify(this.items[0]));
      firstItem.id = this.items.length + 1;
      this.items.push(firstItem);
    },
    slideStart() {
      // 不超过一张图时不滑动
      if (this.items.length <= 1) {
        return;
      }
      // 第一张图切换不计算滑动时间
      setTimeout(() => {
        this.currentItemId += 1;
        // 第二张图以后切换时间为滑动时间+停留时间
        this.slideInterval = setInterval(() => {
          // 循环
          if (this.currentItemId === this.items.length) {
            this.transitionTime = 0;
            this.currentItemId = 1;
            setTimeout(() => {
              this.transitionTime = this.slideTime;
              this.currentItemId += 1;
            }, 0);
          } else {
            this.currentItemId += 1;
          }
        }, this.stayTime + this.slideTime);
      }, this.stayTime);
    },
    slideStop() {
      clearInterval(this.slideInterval);
    },
    handleItemClick(item) {
      if (item.toUrl) {
        window.location.href = item.toUrl;
      }
    },
  },
};
</script>
<style lang="scss">
  .banner-wrapper {
    overflow: hidden;
  }
  .items-container {
    height: 100%;
    display: flex;
    flex-direction: row;
  }
</style>
