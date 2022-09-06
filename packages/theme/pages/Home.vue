<template>
  <div id="home">
    <EziBanner
      link="/offre-de-remboursement"
      image="/homepage/banniere-odr-homepage.jpeg"
    />

    <div class="grey-bg">
      <EziSubtitle
        :title="$t('home_accroche1')"
      />

      <EziCardsRow
        :items="categories"
        itemKey="produit"
        :itemStyle="'thumbnail'"
      />
    </div>

    <EziAccessoriesRow />
    <EziServicesRow />

    <div class="carousels full-width">
      <EziCarousel id="left_carousel" :slides="bottomSlides.leftSlides" />
      <EziCarousel id="right_carousel" :slides="bottomSlides.rightSlides" />
    </div>
    <EziSubtitle 
      :title="$t('home_accroche2')"
    />
    <EziBanner link="https://ezilight.com/" image="/homepage/ezilight.png" />
  </div>
</template>
<script>
import { onSSR } from "@vue-storefront/core";
import {
  app,
  computed,
  useRoute,
  useContext,
} from "@nuxtjs/composition-api";
import { useContent } from '@wiboo/ezishop';
import EziAccessoriesRow from "~/components/EziAccessoriesRow";
import EziBanner from "~/components/EziBanner";
import EziCardsRow from "~/components/EziCardsRow";
import EziCarousel from "~/components/EziCarousel";
import EziServicesRow from "~/components/EziServicesRow";
import EziSubtitle from "~/components/EziSubtitle";

export default {
  name: "Home",
  components: {
    EziAccessoriesRow,
    EziBanner,
    EziCardsRow,
    EziCarousel,
    EziServicesRow,
    EziSubtitle,
  },
  setup(app) {
    const { app: { i18n }, redirect } = useContext() ;
    const locale = i18n.locales.find(({ iso, code }) => [iso, code].includes(i18n.locale)) || {};
    const route = useRoute();
    
    const { resHomeCategories, getHomeCategories, resHomeBottomSlides, getHomeBottomSlides } = useContent();

    onSSR(async () => {
      await getHomeCategories();
      await getHomeBottomSlides();
    });

    console.log('resHomeBottomSlides', resHomeBottomSlides.value)

    return {
      locale,
      categories: resHomeCategories,
      bottomSlides: resHomeBottomSlides,
    };
  },
  data() {
    return {
      carousels: {
        leftCarousel: [
          "/homepage/left-carousel/BlasterF130-4-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-5-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-7-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-10-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-11-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-15-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-16-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-17-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-19-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-20-scaled.jpg",
          "/homepage/left-carousel/BlasterF130-21-scaled.jpg",
        ],

        rightCarousel: [
          "/homepage/right-carousel/CyclowashP420-1-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-2-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-4-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-5-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-6-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-8-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-9-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-10.jpg",
          "/homepage/right-carousel/CyclowashP420-11-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-13-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-14-scaled.jpg",
          "/homepage/right-carousel/CyclowashP420-19-scaled.jpg",
        ],
      },
    };
  },
};
</script>

<style lang="scss" scoped>
.sf-banner {
  --banner-width: 100vw;
  --banner-height: unset;
  --banner-background-position: 100%;
  --banner-display: block;
}

.carousels {
  display: flex;
  justify-content: space-between;
}

.grey-bg {
  background-color: rgb(245, 245, 245);
  padding-top: 1px;
  padding-bottom: 1px;

  .ezi-row-card {
    margin: 50px;
  }
}

@media screen and (max-width: 768px) {
  .sf-heading h3 {
    --heading-title-font-size: 32px;
  }
}

@media screen and (max-width: 425px) {
  .grey-bg {
    .ezi-row-card {
      margin: 25px;
    }
  }
  
  .carousels {
    display: block;

    #right_carousel {
      display: none;
    }
  }
}
</style>
