<template>
  <div class="carousel-container">
    <SfCarousel :settings="settings">
      <template #next>
        <span></span>
      </template>
      <template #prev>
        <span></span>
      </template>
      <SfCarouselItem v-for="(slide, i) in slides" :key="i">
        <slot name="item">
          <img :src="slide.img" class="carousel-img" :alt="slide.title" />
        </slot>
      </SfCarouselItem>
    </SfCarousel>
  </div>
</template>

<script>
import { SfCarousel } from "@storefront-ui/vue";

export default {
  props: {
    slides: {
      type: Array,
      required: true,
    },
    settings: {
      type: Object,
      required: false,
      default: () => {
        return {
          type: "carousel",
          rewind: true,
          perView: 1,
          gap: 0,
          autoplay: 3000,
          animationDuration: 1000,
          breakpoints: { 1023: {}, 768: { perView: 1 } },
          animationTimingFunc: "ease-in-out",
          hoverpause: false,
        };
      },
    },
  },
  components: {
    SfCarousel,
  },
};
</script>

<style lang="scss" scoped>
.sf-carousel {
  --carousel-width: 49vw;

  &-item,
  .carousel-img {
    width: 49vw !important;
  }
}

@media screen and (max-width: 425px) {
  .sf-carousel {
    --carousel-width: 100vw;
    margin-bottom: 10px;

    &-item,
    .carousel-img {
      width: 100vw !important;
    }
  }
}
</style>
