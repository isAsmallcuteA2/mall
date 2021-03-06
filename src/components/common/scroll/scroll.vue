<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      scroll,
      message: "哈哈哈"
    };
  },
  methods: {
    refresh(){
     this.scroll&&this.scroll.refresh();
    },
    back(x, y, time = 500) {
      this.scroll.scrollTo(0, 0, time);
    },
    finishPullUp(){
      this.scroll.finishPullUp();
    },
    // 获取Y's height
    getScrollY(){
      return this.scroll?this.scroll.y:0
    }
  },
  mounted() {
    // 1.创建BScroll对象
    this.scroll = new BScroll(this.$refs.wrapper, {
      click: true,
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad
    }),
      // 监听滚动位置
      this.scroll.on("scroll", position => {
        // console.log(position);
        this.$emit("scroll", position);
      })
      // this.scroll.on("pullingUp", () => {
      //   // console.log("loadmore");
      //   this.$emit("pullingUp")
      // })
       if (this.pullUpLoad) {
        this.scroll.on('pullingUp', () => {
          this.$emit('pullingUp')
        })
      }}
};

// 2.监听滚动的位置
//       if (this.probeType === 2 || this.probeType === 3) {
//         this.scroll.on('scroll', (position) => {
//           // console.log(position);
//           this.$emit('scroll', position)
//         })
//       }

//       // 3.监听scroll滚动到底部
//       if (this.pullUpLoad) {
//         this.scroll.on('pullingUp', () => {
//           this.$emit('pullingUp')
//         })
//       }
//     },
//     methods: {
//       scrollTo(x, y, time=300) {
//         this.scroll && this.scroll.scrollTo(x, y, time)
//       },
//       refresh() {
//         this.scroll && this.scroll.refresh()
//       },
//       finishPullUp() {
//         this.scroll && this.scroll.finishPullUp()
//       },
//       getScrollY() {
//         return this.scroll ? this.scroll.y : 0
//       }
// }
</script>

<style scoped></style>
