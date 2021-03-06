<template>
  <div class="tabcontrol">
    <div
      v-for="(item, index) in titles"
      :key="item"
      class="itemclass"
      :class="{ active: index === currentIndex }"
      @click="tabClick(index)"
    >
      <span>{{ item }}</span>
    </div>
  </div>
</template>
<script>
export default {
  name: "TabControl",
  data() {
    return {
      currentIndex: 0
    };
  },
  methods: {
    tabClick(index) {
      this.currentIndex = index;
      this.$emit("TabClick",index)
    }
  },
  props: {
    titles: {
      type: Array,
      default() {
        return [];
      }
    }
  }
};
</script>
<style>
.tabcontrol {
  display: flex;
  height: 40px;
  line-height: 40px;
  text-align: center;
  font-size: 15px;
  background: #fff;
  z-index: 99;
}
.itemclass {
  flex: 1;
}

.itemclass span {
  padding: 4px;
}

.itemclass.active span {
  border-bottom: 3px solid pink;
}
.tabcontrol {
  /* position: sticky;
  top: 44px; */
  background: #fff;
}
</style>
