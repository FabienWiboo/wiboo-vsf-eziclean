<template>
  <div class="subtitle full-width">
    <div class="h-separator"></div>
    <EziTitle class="subtitle-content" :level="level" :title="title" />
    <div class="h-separator"></div>
  </div>
</template>

<script>
import EziTitle from "~/components/EziTitle";

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
      default: 4,
    },
  },
  components: {
    EziTitle,
  },
};
</script>

<style scoped lang="scss">
.h-separator {
  flex: 1 1 auto;
}

.subtitle {
  display: flex;
  width: 100%;
  align-items: center;
  margin: 40px 0;

  & ::v-deep .sf-heading__title {
    font-size: 40px;
    text-align: center;
    line-height: 1;
    margin: 0 80px;
  }

  &-content {
    width: max-content;
    overflow-wrap: break-word;
  }
}

@media screen and (max-width: 768px) {
  .subtitle .subtitle-content::v-deep {
    .sf-heading__title {
      font-size: 32px;
    }
  }
}
</style>
