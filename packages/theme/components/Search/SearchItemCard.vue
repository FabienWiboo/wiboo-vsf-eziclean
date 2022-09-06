<template>
  <component
    :is="'div'"
    ref="test"
    :href="isLink()"
    class="ezi-card category-style"
  >
    <figure v-if="imageLink">
      <SfImage class="image" :src="imageLink" alt="Image" />
    </figure>
    <div>
      <p class="title">
        <slot name="title"></slot>
      </p>
    </div>
    <div class="card-details">
      <SfHeading class="price" :level="4" :title="price | currency" />
      <SfLink
        :href="cardLink"
        class="button-link"
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
    isLink() {
      return this.cardLink;
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
    padding: 0 15px 15px 15px;
    box-shadow: rgb(0 0 0 / 5%) 0 0 5px 3px;
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
      font-size: 16px;
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
        width: 25vw;
        height: 20vw;
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
    line-height: 1.2;
    font-size: 18px;
  }

  .card-details {
    margin: auto 0 0 0;

    .price {
      margin-bottom: 20px;

      .h4 {
        font-size: 22px;
      }
    }
  }

  ::v-deep img {
    height: 150px;
  }
}
</style>