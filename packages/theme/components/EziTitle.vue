<template>
  <div class="ezi-title">
    <SfHeading class="title-content"  :class="'align-'+align" :level="level" :title="title" />
  </div>
</template>

<script>
import { SfHeading } from "@storefront-ui/vue";

export default {
  props: {
    title: {
      type: String,
      required: true,
      default: 'Veuillez entrer un titre {prop: "title"}',
    },
    level: {
      type: Number,
      required: false,
      default: 1,
    },
    align: {
      type: String,
      default: "center",
    },
  },
  components: {
    SfHeading,
  },
};
</script>

<style scoped lang="scss">
.title ::v-deep {
  text-align: center;
  margin: auto 50px;
  width: 90vw;

  h1.sf-heading__title {
    line-height: 0.9;
    font-size: 60px;
  }

  h4.sf-heading__title {
    font-size: 40px;
  }
}

.align-left {
  text-align: left;
}

.align-center {
  text-align: center;
}

.align-right {
  text-align: right;
}
</style>
