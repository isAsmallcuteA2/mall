<template>
  <div id="detail">
    <!-- <h2>Detail</h2> -->
    <!-- <p>{{this.$route.params.id}}</p> -->
    <top-bar class="detail-nav"/>
    <scroll class="content" ref="scroll">
      <detail-swiper :top-images="topImages" />
      <detail-base-info :goods="goods" />
      <detail-shop-info :shop="shop" />
      <detail-goods-info :detail-info="detailInfo" @imageLoad="imageLoad" />
      <detail-param-info :param-info="paramInfo" />
    </scroll>
  </div>
</template>
<script>
import topBar from "views/detail/childCpn/topBar";
import { getDetail } from "network/detail.js";
import detailSwiper from "views/detail/childCpn/detailSwiper";
import DetailBaseInfo from "./childCpn/DetailBaseInfo";
import DetailShopInfo from "./childCpn/DetailShopInfo";
import DetailGoodsInfo from "./childCpn/DetailGoodsInfo";
import DetailParamInfo from "./childCpn/DetailParamInfo";
import Scroll from "components/common/scroll/scroll";
import { Goods, Shop, GoodsParam } from "network/detail";

export default {
  data() {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {}
    };
  },
  components: {
    topBar,
    detailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    DetailGoodsInfo,
    DetailParamInfo,
    Scroll
  },
  created() {
    this.iid = this.$route.params.id;
    getDetail(this.iid).then(res => {
      // 1.获取顶部的图片轮播数据
      console.log(res);
      const data = res.result;
      this.topImages = data.itemInfo.topImages;

      // 2.获取商品信息
      this.goods = new Goods(
        data.itemInfo,
        data.columns,
        data.shopInfo.services
      );

      // 3.创建店铺信息的对象
      this.shop = new Shop(data.shopInfo);

      // 4.保存商品的详情数据
      this.detailInfo = data.detailInfo;

      // 5.获取参数的信息
      this.paramInfo = new GoodsParam(
        data.itemParams.info,
        data.itemParams.rule
      );
    });
  },
  methods: {
    imageLoad() {
      this.$refs.scroll.refresh();
    }
  }
};
</script>
<style scoped>
/* #detail{
  position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
}
.detail-nav{
  position: relative;
  z-index: 9;
  background: #fff;
}
.content{
  height: calc(100%-44px);
} */
#detail {
    position: relative;
    z-index: 9;
    background-color: #fff;
    height: 100vh;
  }

  .detail-nav {
    position: relative;
    z-index: 9;
    background-color: #fff;
  }

  .content {
    height: calc(100% - 44px);
  }
</style>
