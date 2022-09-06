<template>
  <div class="ezi-row-card" :class="`${itemStyle}-style`">
    <EziItemCard
      v-for="(item, i) in items"
      :key="`${itemKey}-${i}`"
      :imageLink="item.img"
      :price="item.price"
      :cardLink="item.link"
      :itemStyle="itemStyle"
    >
      <template #title>{{ item.title }}</template>
      <template #content>{{ item.content }}</template>
    </EziItemCard>
  </div>
</template>

<script>
import EziItemCard from "~/components/EziItemCard";

export default {
  props: {
    items: {
      type: Array,
      required: true,
    },
    itemKey: {
      type: String,
      default: "item",
    },
    itemStyle: {
      type: String,
      default: "product",
    },
  },
  components: {
    EziItemCard,
  },
};
</script>

<style lang="scss" scoped>
.ezi-row-card {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 35px;
}

@media screen and (max-width: 425px) {
  .ezi-row-card.thumbnail-style {
    grid-template-columns: 1fr 1fr;
    gap: 10px;
  }

  .ezi-row-card.product-style {
    display: grid;
    grid-template-columns: 1fr;
    gap: 35px;
  }
}

@media screen and (min-width: 1440px) {
  .ezi-row-card.product-style {
    grid-template-columns: repeat(3, 1fr);
    gap: 3vw;
  }

  .ezi-row-card.thumbnail-style {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>
