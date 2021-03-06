<template>
  <swiper>
    <swiper-item v-for="item in banners" :key="item.link">
      <a :href="item.link">
        <img :src="item.image" alt="" @load="imageLoad()" />
      </a>
    </swiper-item>
  </swiper>
</template>

<script>
import { Swiper, SwiperItem } from "components/common/swiper";

export default {
  data(){
    return{
          isLoad:false

    }
  },
  name: "HomeSwiper",
  props: {
    banners: {
      type: Array,
      default: () => []
    }
  },
  components: {
    Swiper,
    SwiperItem
  },
  methods: {
    imageLoad() {
      if(!this.isLoad){
        this.$emit("swiperLoad");
        this.isLoad=true
      }
    }
  }
};
</script>

<style scoped></style>
