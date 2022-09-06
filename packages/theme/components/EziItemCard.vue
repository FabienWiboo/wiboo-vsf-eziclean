<template>
  <component
    :is="getItemTag()"
    ref="test"
    :href="isLink()"
    class="ezi-card"
    :class="{
      'category-style': itemStyle == 'product',
      'thumbnail-style': itemStyle == 'thumbnail',
    }"
  >
    <figure v-if="imageLink">
      <SfImage class="image" :src="imageLink" alt="Image" />
    </figure>
    <div>
      <p class="title">
        <slot name="title"></slot>
      </p>
      <p class="content" v-if="itemStyle == 'product'">
        <slot name="content"></slot>
      </p>
    </div>
    <div class="card-details" v-if="itemStyle == 'product'">
      <SfHeading class="price" :level="4" :title="price | currency" />
      <SfLink
        :href="cardLink"
        class="button-link"
        v-if="itemStyle == 'product'"
      >
        <SfButton class="button-label">{{ buttonLabel }}</SfButton>
      </SfLink>
    </div>
  </component>
</template>

<script>
import { SfImage, SfLink, SfHeading, SfButton } from "@storefront-ui/vue";

export default {
  props: {
    itemStyle: {
      type: String,
      default: "product",
    },
    imageLink: {
      type: String,
      required: true,
    },
    imageAlt: {
      type: String,
      default: "Image",
    },
    price: {
      type: Number,
      default: 99.99,
    },
    buttonLabel: {
      type: String,
      default: "Découvrir",
    },
    cardLink: {
      type: String,
    },
  },
  components: {
    SfImage,
    SfLink,
    SfHeading,
    SfButton,
  },
  methods: {
    getItemTag() {
      if (this.itemStyle == "product") return "div";
      else if (this.itemStyle == "thumbnail") return "a";
    },
    isLink() {
      if (this.getItemTag() == "a") {
        return this.cardLink;
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.ezi-card {
  text-align: center;
  display: flex;
  flex-direction: column;

  &.category-style {
    padding: 0 30px 30px 30px;
    box-shadow: rgb(0 0 0 / 10%) 0 0 10px 6px;
  }

  &.thumbnail-style::v-deep {
    font-weight: 600;
    margin: auto;
    width: 300px;

    img:not(.display-none) {
      object-fit: cover;
      height: 200px;
      display: block;
      margin: auto;
      width: 300px;
    }

    p {
      margin-top: 10px;
      font-size: 22px;
      white-space: nowrap;
    }
  }

  @media screen and (max-width: 425px) {
    &.thumbnail-style::v-deep {
      font-weight: 600;
      margin: auto;
      width: 40vw;
      height: 175px;

      img {
        object-fit: cover;
        display: block;
        margin: auto;
        width: 40vw;
        height: 30vw;
      }

      p {
        margin-top: 5px;
        font-size: 18px;
        white-space: pre-wrap;
        line-height: 1;
      }
    }
  }

  .title {
    line-height: 1.6;
    font-size: 24px;
  }

  .content {
    margin: 10px 0px;
    line-height: 1.6;
  }

  .card-details {
    margin: auto 0 0 0;

    .price {
      margin-bottom: 30px;

      .h4 {
        font-size: 28px;
      }
    }
  }

  ::v-deep img {
    height: 250px;
  }
}
</style>