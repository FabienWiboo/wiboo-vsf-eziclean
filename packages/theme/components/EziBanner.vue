<template>
  <div class="ezi-banner full-width">
    <slot name="beforeBanner"></slot>
    <SfLink v-show="link" :link="link">
      <SfImage v-show="image" :alt="alt" width="100%" :src="image"  />
      <div class="video-container full-width" v-show="video">
        <video autoplay="" muted="" loop="" width="100%" height="450">
          <source :src="video" />
        </video>
      </div>
    </SfLink>
    <div v-show="!link">
      <SfImage v-show="image" :alt="alt" loading="eager" width="100%" :src="image"/>
      <div class="video-container" v-show="video">
        <video autoplay="" muted="" loop="" width="100%" height="450">
          <source :src="video" />
        </video>
      </div>
    </div>
    <slot name="afterBanner"></slot>
  </div>
</template>

<script>
import { SfLink, SfImage } from "@storefront-ui/vue";

export default {
  components: {
    SfLink,
    SfImage,
  },
  props: {
    link: {
      type: String,
      required: false,
      default: "",
    },
    image: {
      type: String,
      required: false,
      default: "",
    },
    video: {
      type: String,
      required: false,
      default: "",
    },
    alt: {
      type: String,
      required: false,
      default: "Media",
    },
  },
};
</script>

<style lang="scss" scoped>
.video-container {
  video {
    object-fit: cover;
  }
}
.ezi-banner .sf-image--wrapper {
  width: 100%;
}
</style>