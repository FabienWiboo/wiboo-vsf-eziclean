<template>
  <div class="ezi-accessory--container">
    <SfLink :href="cardLink" class="ezi-accessory">
      <SfImage
        v-show="imageLink"
        class="image"
        :src="imageLink"
        alt="Image"
        loading="eager"
      />
      <p class="title">
        {{ title }}
      </p>
      <SfHeading class="price" :level="3" :title="price" />
    </SfLink>
  </div>
</template>

<script>
import { SfImage, SfLink, SfHeading } from "@storefront-ui/vue";

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
      type: String,
      default: "99.99€",
    },
    cardLink: {
      type: String,
    },
    title: {
      type: String,
      required: true,
    },
  },
  components: {
    SfImage,
    SfLink,
    SfHeading,
  },
};
</script>

<style lang="scss" scoped>
.ezi-accessory {
  text-align: center;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-direction: column;
  box-shadow: rgb(0 0 0 / 10%) 0 0 10px 6px;
  margin: 1em;
  padding: 12px;

  .image::v-deep img {
    height: 145px;
    object-fit: contain;
    width: 100%;
    margin-bottom: 25px;
  }

  .title {
    font-size: 18px;
  }

  .price .sf-heading__title.h3 {
    font-size: 20px;
    font-weight: 600;
  }
}

@media screen and (max-width: 1024px) {
  .ezi-accessory {
    min-width: 125px;
    max-width: 70vw;

    .image::v-deep img {
      height: 80px;
      object-fit: contain;
      width: 100%;
      margin-bottom: 25px;
    }
  }
}

@media screen and (max-width: 425px) {
  .ezi-accessory {
    margin-left: auto;
    margin-right: auto;
  }
}
</style>
