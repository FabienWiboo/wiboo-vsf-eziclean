<template>
  <SfLink href="/accessoires" class="ezi-accessories-row">
    <SfImage src="/accessories/brosses.jpg" alt="Brosse"></SfImage>
    <SfHeading :level="3">
      <template #title>
        <h3 class="sf-heading__title h3" v-html="$t('accessory_banner_title')"></h3>
      </template>
    </SfHeading>
  </SfLink>
</template>

<script>
import { SfImage, SfLink, SfHeading } from "@storefront-ui/vue";

export default {
  components: {
    SfImage,
    SfLink,
    SfHeading,
  },
};
</script>

<style lang="scss" scoped>
.ezi-accessories-row::v-deep {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  justify-content: space-evenly;
  padding: 12px 0;

  img {
    width: 500px;
    max-width: 90vw;
  }

  .sf-heading {
    &__title {
      font-weight: 400;
      width: max-content;
      letter-spacing: -1px;
    }

    & h3 {
      --heading-title-font-size: 38px;
      max-width: 90vw;
    }
  }

  .sf-header-navigation-item ::v-deep .sf-menu-item__label {
    --menu-item-label-color: black;
  }
}

@media screen and (max-width: 1440px) {
  .ezi-accessories-row .sf-heading h3 {
    --heading-title-font-size: 32px;
  }
}
</style>
