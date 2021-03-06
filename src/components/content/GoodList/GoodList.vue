<template>
  <div class="goods">
    <!-- {{goods}} -->

    <goods-list-item v-for="item in goods" :goods-item="item" :key="item.id">
    </goods-list-item>
  </div>
</template>
<script>
// import GoodList from "components/content/GoodList/GoodList";

import GoodsListItem from "components/content/GoodListItem/goodslistitem";
export default {
  props: {
    goods: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  components: {
    GoodsListItem
  }
};
</script>
<style scoped>
.goods {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;

  padding: 2px;
}
</style>
