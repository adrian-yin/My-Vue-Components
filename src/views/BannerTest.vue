<template>
  <div class="banner-test">
    <Banner
      :banner-height="10"
      :banner-width="15"
      :slide-time="500"
      :slide-mode="'ease-in-out'"
      :stay-time="500"
      :pic-items="picItems"
    >
    </Banner>
  </div>
</template>
<script>
import Banner from '@/components/Banner.vue';

const img1 = require('@/assets/img/1.jpg');
const img2 = require('@/assets/img/2.jpg');
const img3 = require('@/assets/img/3.jpg');
const img4 = require('@/assets/img/4.jpg');
const img5 = require('@/assets/img/5.jpg');
const img6 = require('@/assets/img/6.jpg');
const img7 = require('@/assets/img/7.jpg');

export default {
  name: 'BannerTest',
  components: {
    Banner,
  },
  data() {
    return {
      picItems: [
        {
          id: 1,
          imgUrl: img1,
          imgAlt: '',
          toUrl: 'https://www.baidu.com',
        },
        { id: 2, imgUrl: img2, imgAlt: '' },
        { id: 3, imgUrl: img3, imgAlt: '' },
        { id: 4, imgUrl: img4, imgAlt: '' },
        { id: 5, imgUrl: img5, imgAlt: '' },
        { id: 6, imgUrl: img6, imgAlt: '' },
        { id: 7, imgUrl: img7, imgAlt: '' },
      ],
    };
  },
};
</script>
<style lang="scss">
  .banner-test {
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
