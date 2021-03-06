<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <tab-control
      ref="tabcontrol2"
      class="tab-control"
      v-show="isShow"
      :titles="['流行', '新款', '经典']"
      @TabClick="TabClick"
    />
    <!-- 判断是否监听 -->
    <scroll
      class="content"
      :pullUpLoad="true"
      @pullingUp="loadMore"
      ref="scroll"
      :probe-type="3"
      @scroll="scroll"
    >
      <home-swiper :banners="banners" @swiperLoad="swiperLoad" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control
        ref="tabcontrol1"
        :titles="['流行', '新款', '经典']"
        @TabClick="TabClick"
      />
      <good-list :goods="chooseLike" />
    </scroll>
    <back-to-top @click.native="backToTop" v-show="isBackShow" />
  </div>
</template>

<script>
// son
import NavBar from "components/common/navbar/navBar";
import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";
// public
import TabControl from "components/content/TabControl/TabControl";
import GoodList from "components/content/GoodList/GoodList";
import scroll from "components/common/scroll/scroll";
import BackToTop from "components/common/BackTop/BackTop";

// function
import { getHomeMultidata, getHomeGoods } from "network/home";
import { debounce } from "common/utils";
export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodList,
    scroll,
    BackToTop
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      currentIndex: "pop",
      isBackShow: false,
      taboffsetTop: 0,
      isShow: false,
      scrollY:0
    };
  },
  computed: {
    chooseLike() {
      return this.goods[this.currentIndex].list;
    }
  },
  created() {
    //  swiper
    this.getHomeMultidata();
    this.getHomeGoods("sell");
    this.getHomeGoods("new");

    this.getHomeGoods("pop");
  },
  mounted() {
    // 出发refresh函数
    const refresh = debounce(this.$refs.scroll.refresh, 500);

    this.$bus.$on("itemImageLoad", () => {
      // console.log(1)
      refresh();
    });
  },
  // 组件销毁
  destroyed() {
    console.log("组件销毁");
  },
  activated() {
    // console.log("active");
    this.$refs.scroll.scroll.scrollTo(0,this.scrollY,0)
  },
  deactivated() {
    // console.log("deactived");
    this.scrollY = this.$refs.scroll.getScrollY();
  },
  methods: {
    swiperLoad() {
      console.log(this.$refs.tabcontrol1.$el.offsetTop);
      this.taboffsetTop = this.$refs.tabcontrol1.$el.offsetTop;
    },
    // loadmore
    loadMore() {
      // console.log("loadmore")
      this.getHomeGoods(this.currentIndex);
      this.$refs.scroll.finishPullUp();
      this.$refs.scroll.refresh();
    },
    scroll(position) {
      // if((-position.y)>1000){
      //   this.isBackShow== true
      // }
      // console.log(isBackShow)
      this.isBackShow = -position.y > 1000;
      this.isShow = -position.y > this.taboffsetTop;
    },
    backToTop() {
      // this.$refs.scroll.scroll.scrollTo(0,0,500)
      this.$refs.scroll.back();
    },
    // 事件监听相关方法
    TabClick(index) {
      console.log(index);
      switch (index) {
        case 0:
          this.currentIndex = "pop";
          break;
        case 1:
          this.currentIndex = "new";
          break;
        case 2:
          this.currentIndex = "sell";
          break;
      }
      // let index at the same code
      this.$refs.tabcontrol1.currentIndex = index;
      this.$refs.tabcontrol2.currentIndex = index;
    },
    // data's request
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        console.log(res);
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
        console.log(this.goods[type].list);
      });
    }
    // debounce(func, delay) {
    //   let timer = null;
    //   return function(...args) {
    //     if (timer) clearTimeout(timer);
    //     timer = setTimeout(() => {
    //       func.apply(this, args);
    //     }, delay);
    //   };
    // }
  }
};
</script>

<style scoped>
#home {
  /*padding-top: 44px;*/
  height: 100vh;
  /* position: relative; */
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  /*在使用浏览器原生滚动时, 为了让导航不跟随一起滚动*/
  /*position: fixed;*/
  /*left: 0;*/
  /*right: 0;*/
  /*top: 0;*/
  /*z-index: 9;*/
}

.content {
  overflow: hidden;

  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}

.tab-control {
  position: relative;
  z-index: 9;
  margin-top: 0px;
}
</style>
