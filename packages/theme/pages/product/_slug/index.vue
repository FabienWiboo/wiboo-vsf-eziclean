<template>
  <div id="product" class="page-margin">
    <EziBreadcrumb :paths="product.paths" v-if="product && product.paths" />

    <EziProductSummary :product="product">
      <template #addToCartButton>
        <SfAddToCart
          v-e2e="'product_add-to-cart'"
          stock="100"
          v-model="qty"
          :disabled="loading"
          :canAddToCart="true"
        >
        <template #add-to-cart-btn>
          <SfButton
          class="add-to-cart"
          :disabled="loading"
          @click="addItem({ product, quantity: parseInt(qty) })"
        >
        {{ $t('add_to_cart') }}
        </SfButton>
        </template>
        </SfAddToCart>
      </template>
    </EziProductSummary>
    <EziServicesRow class="ignore-page-margin" />
    <div
      ref="productTabsFakeSpace"
      :style="`height: ${isStickyProductTabs ? productTabsHeight : 0}px;`"
    ></div>
    <div
      class="product-tabs"
      ref="productTabs"
      :class="{
        sticky: isStickyProductTabs,
        'menu-active': scrollToBottom,
      }"
    >
      <ul>
        <li v-show="boxBannerImage">
          <a href="#box_content" @click="goToAnchor">{{ $t("box_content") }}</a>
        </li>
        <li
          v-show="product.content && product.content.technologies"
        >
          <a href="#technologies" @click="goToAnchor">{{
            $t("technologies")
          }}</a>
        </li>
        <li v-show="product.caracteristics[0]">
          <a href="#caracteristics" @click="goToAnchor">{{
            $t("caracteristics")
          }}</a>
        </li>
        <li v-show="product.accessories[0]">
          <a href="#accessories" @click="goToAnchor">{{ $t("accessories") }}</a>
        </li>
        <li v-show="product.detachedPieces[0]">
          <a href="#detached_pieces" @click="goToAnchor">{{
            $t("detached_pieces")
          }}</a>
        </li>
      </ul>
    </div>
    <div class="ignore-page-margin box-content" v-show="boxBannerImage">
      <EziBanner
        class="page-margin"
        :image="boxBannerImage"
        :alt="$t('box_content')"
        id="box_content"
      >
        <template #beforeBanner>
          <EziTitle :title="$t('box_content')" align="left" :level="3" />
        </template>
      </EziBanner>
    </div>
    <div
      class="product-technologies"
      id="technologies"
      v-if="product.content && product.content.technologies"
    >
      <EziTitle :title="$t('technologies')" align="left" :level="3" />
      <div
        class="technologies"
        v-if="product.content.technologies"
      >
      <div v-for="technology in product.content.technologies">
        <div v-for="block in technology.Contenu.blocks">
          <div v-if="block.type=='image'">
              <img :src="prefix_url_directus + block.data.file.url" :alt="block.data.caption" />
          </div>
          <div v-if="block.type=='header'">
            <h3>{{ block.data.text }}</h3>
          </div>
          <div v-if="block.type=='paragraph'" :v-html="block.data.text"></div>
        </div>
      </div>
      </div>
    </div>
    <div
      class="product-caracteristics"
      id="caracteristics"
      v-if="product.caracteristics[0]"
    >
      <EziTitle :title="$t('caracteristics')" align="left" :level="3" />
      <div class="caracteristics">
        <div v-for="(caracteristic, i) in product.caracteristics" :key="i">
          <p class="title">{{ caracteristic.title }}</p>
          <p class="content">{{ caracteristic.content }}</p>
        </div>
      </div>
    </div>
    <div
      class="product-accessories"
      id="accessories"
      v-if="product.accessories[0]"
    >
      <EziTitle :title="$t('accessories')" align="left" :level="3" />
      <div class="accessories">
        <EziSlider
          :options="{
            slidesToShow: 1,
            infinite: product.accessories.length > 6 ? true : false,
            dots: false,
            responsive: [
              {
                breakpoint: 425,
                settings: {
                  slidesToShow: 2,
                },
              },
              {
                breakpoint: 768,
                settings: {
                  slidesToShow: 3,
                },
              },
              {
                breakpoint: 1024,
                settings: {
                  slidesToShow: 6,
                },
              },
            ],
          }"
          class="agile-slider"
          :class="{
            'accessories-limit-width': product.accessories.length <= 6,
          }"
        >
          <EziAccessoryCard
            :key="`accessory-${i}`"
            v-for="(accessory, i) in product.accessories"
            :title="accessory.title"
            :imageLink="accessory.img"
            :price="accessory.price | currency"
            :cardLink="accessory.link"
            class="slide"
          />
          <template slot="prevButton"
            ><div
              class="prev-button"
              :class="{
                'desktop-hide-slider-buttons': product.accessories.length <= 6,
                'mobile-hide-slider-buttons': product.accessories.length < 2,
              }"
            ></div
          ></template>
          <template slot="nextButton"
            ><div
              class="next-button"
              :class="{
                'desktop-hide-slider-buttons': product.accessories.length <= 6,
                'mobile-hide-slider-buttons': product.accessories.length < 2,
              }"
            ></div
          ></template>
        </EziSlider>
      </div>
    </div>
    <div
      class="product-detached-pieces"
      id="detached_pieces"
      v-if="product.detachedPieces[0]"
    >
      <EziTitle :title="$t('detached_pieces')" align="left" :level="3" />
      <div class="detached-pieces">
        <EziSlider
          :options="{
            slidesToShow: 1,
            dots: false,
            infinite: product.detachedPieces.length > 6 ? true : false,
            responsive: [
              {
                breakpoint: 1024,
                settings: {
                  slidesToShow: 6,
                },
              },
            ],
          }"
          class="agile-slider"
        >
          <EziAccessoryCard
            v-for="(detachedPiece, i) in product.detachedPieces"
            :key="'detached_piece-' + i"
            :title="detachedPiece.title"
            :imageLink="detachedPiece.img"
            :price="detachedPiece.price | currency"
            :cardLink="detachedPiece.link"
          />
          <template slot="prevButton"
            ><div
              class="prev-button"
              :class="{
                'desktop-hide-slider-buttons':
                  product.detachedPieces.length <= 6,
                'mobile-hide-slider-buttons': product.detachedPieces.length < 2,
              }"
            ></div
          ></template>
          <template slot="nextButton"
            ><div
              class="next-button"
              :class="{
                'desktop-hide-slider-buttons':
                  product.detachedPieces.length <= 6,
                'mobile-hide-slider-buttons': product.detachedPieces.length < 2,
              }"
            ></div
          ></template>
        </EziSlider>
      </div>
    </div>
  </div>
</template>
<script>
import { onSSR } from "@vue-storefront/core";
import { SfAddToCart, SfButton } from "@storefront-ui/vue";
import {
  ref,
  app,
  computed,
  useRoute,
  useMeta,
  useContext,
} from "@nuxtjs/composition-api";
import { useProduct, useCart, productGetters } from "@wiboo/ezishop";
import EziBreadcrumb from "~/components/EziBreadcrumb";
import EziProductSummary from "~/components/EziProductSummary";
import EziServicesRow from "~/components/EziServicesRow";
import EziBanner from "~/components/EziBanner";
import EziTitle from "~/components/EziTitle";
import EziAccessoryCard from "~/components/EziAccessoryCard";
import EziSlider from "~/components/EziSlider/EziSlider";

export default {
  name: "Product",
  components: {
    EziBreadcrumb,
    EziProductSummary,
    EziServicesRow,
    EziBanner,
    EziTitle,
    EziAccessoryCard,
    SfAddToCart,
    SfButton,
    EziSlider,
  },
  setup(app) {
    console.log('SETUP');
    const {
      app: { i18n },
      redirect,
    } = useContext();
    const locale =
      i18n.locales.find(({ iso, code }) => [iso, code].includes(i18n.locale)) ||
      {};
    const route = useRoute();
    const qty = ref(1);
    const { addItem, loading } = useCart();
    const { products, search } = useProduct("products");
    const slug = computed(() => route.value.params.slug);
    onSSR(async () => {
      await search({ slug: slug.value });
    });
    const product = computed(() => productGetters.getFiltered(products.value, { locale: locale })[0]);

    console.log('product', product.value);

    if(product && slug.value!=product.permalien){
      const redirect_path = ((locale.code!="fr") ? '/'+locale.code : '')+'/product/'+product.permalien;
      //redirect( redirect_path );
    }

    useMeta(() => ({ title: productGetters.getMetaTitle(product.value) }));
    return {
      locale,
      slug,
      qty,
      addItem,
      loading,
      product,
      productGetters,
    };
  },
  methods: {
    getProductTabsHeight() {
      this.productTabsHeight =
        this.$refs.productTabs?.getBoundingClientRect().height + 40;
    },
    checkStickyProductTabs() {
      let heightLimit = 0;

      if (document.querySelector(".show-menu")) {
        heightLimit = 90;
      }

      if (
        this.$refs.productTabsFakeSpace.getBoundingClientRect().y <= heightLimit
      ) {
        this.isStickyProductTabs = true;
      } else {
        this.isStickyProductTabs = false;
      }
    },
    goToAnchor(e) {
      e.preventDefault();
      const target = e.target.hash;
      const el = document.querySelector(target);
      const position = el.getBoundingClientRect();

      const isMenuGonnaOpen = () => {
        return position.y < 0;
      };

      window.scrollTo({
        top: window.scrollY + position.y - (isMenuGonnaOpen() ? 160 : 60),
        behavior: "smooth",
      });
    },
  },
  computed: {
    scrollToBottom() {
      return this.$store.state.scrollDirection.toBottom;
    },
  },
  mounted() {
    this.getProductTabsHeight();
    window.addEventListener("resize", () => this.getProductTabsHeight());
    this.checkStickyProductTabs();
    window.addEventListener("scroll", () => this.checkStickyProductTabs());

    if (this.product?.content.contenu_de_la_boite) {
      this.boxBannerImage =
        process.env.DIRECTUS_API_URL +
        "/assets/" +
        this.product.content.contenu_de_la_boite.filename_disk;
    }
  },
  /*head() {
    return {
      title: (this.product.content?.meta_title ?? '') + '|' + (process.env.META_SHOP_NAME || "Eziclean"),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: (this.product.content?.meta_description ?? '')
        }
      ],
    }
  },*/
  data() {
    return {
      boxBannerImage: "",
      productTabsHeight: null,
      isStickyProductTabs: false,
      prefix_url_directus: process.env.DIRECTUS_API_URL
    };
  },
};
</script>

<style lang="scss" scoped>
#product {
  --page-width: 1200px;
  --ignore-page-margin: calc((-100vw + var(--page-width)) / 2);

  .add-to-cart {
    background: #118ab2;
    padding: 1em;
    color: #fff;
    margin-right: 1em;
    font-size: 18px;
    border-radius: 3px;
    text-transform:inherit;
  }

  .agile-slider {
    .prev-button,
    .next-button {
      z-index: 1;
      position: absolute;
      font-size: 0;
      background-color: #fff;
      top: 37%;
      box-shadow: 0 1px 12px #0000003b;
      width: 30px;
      height: 85px;
      padding: 0 12px;
      display: block;
      background-repeat: no-repeat;
      background-size: contain;
      background-position: center;
      background-image: url(/theme-icons/slider/slider-button.png);
    }

    &.accessories-limit-width {
      margin: auto;
      width: min-content;

      &::v-deep .slide {
        min-width: 200px;
        max-width: 200px;
      }
    }

    @media screen and (min-width: 1025px) {
      .desktop-hide-slider-buttons {
        display: none;
      }
    }

    @media screen and (max-width: 1024px) {
      .mobile-hide-slider-buttons {
        display: none;
      }
    }
    .prev-button {
      transform: rotate(180deg);
      left: -5%;
    }

    .next-button {
      right: -5%;
    }
  }

  @media screen and (max-width: 1440px) {
    --page-width: 80vw;
    --ignore-page-margin: calc((-100vw + var(--page-width)) / 2);

    .agile-slider {
      .prev-button {
        transform: rotate(180deg);
        left: -4%;
      }

      .next-button {
        right: -4%;
      }
    }
  }

  .product-tabs {
    position: relative;
    left: 0;
    z-index: 49;
    background-color: white;

    &.sticky {
      padding: 20px 0;
      width: 100vw;
      position: fixed;
      top: 0px;
      transform: translateY(90px);
      margin: 0;
      transition: transform 0.3s ease;

      &.menu-active {
        transform: translateY(0px);
      }
    }

    ul {
      display: flex;
      justify-content: center;
      width: 100%;
      flex-wrap: wrap;
      gap: 15px 0;

      li {
        font-size: 1rem;
        border-right: 2px solid rgba(31, 31, 31, 0.6);
        padding: 0 1em;
        font-family: "Source Sans Pro", sans-serif;
        font-weight: 700;
        list-style: none;

        &:last-child {
          border-right: none;
        }

        a {
          color: rgba(30, 30, 30, 0.6);
          transition: all 0.3s ease;
          white-space: nowrap;
        }
      }

      li:hover {
        a {
          color: rgba(30, 30, 30, 1);
        }
      }
    }
  }

  .box-content {
    background-color: #ececec;
    padding: 30px;

    &::v-deep .sf-image {
      width: calc(100% - 80px);
      margin: auto;
    }
  }

  .technologies,
  .caracteristics {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;

    &.technologies {
      gap: 75px;
    }

    &.caracteristics {
      gap: 10px 50px;
    }

    &.technologies::v-deep > div {
      display: flex;
      flex-direction: column;
      gap: 20px;

      img {
        width: 100%;
      }

      p {
        line-height: 1.6;
        color: rgb(85, 85, 85);
        font-size: 16px;
      }
    }

    &.caracteristics > div {
      border-top: 1px solid black;

      .title {
        margin: 8px 0 0 0;
        font-size: 24px;
        font-family: "Source sans Pro", sans-serif;
        font-weight: 400;
        color: #000;
      }

      .content {
        margin: 0 0 20px;
        font-size: 24px;
        font-family: "Source Sans Pro", sans-serif;
        color: #c5c5c5;
        font-weight: 700;
        line-height: 1;
      }
    }
  }

  [class*="product-"] {
    margin-top: 30px;
    margin-bottom: 30px;

    .ezi-title {
      margin-bottom: 20px;
    }
  }
}

@media screen and (max-width: 768px) {
  #product {
    .product-technologies {
      .technologies {
        grid-template-columns: 1fr 1fr;
        gap: 25px;

        & > div {
          gap: 10px;

          p {
            line-height: 1.5;
            font-size: 14px;
          }
        }
      }
    }

    .product-caracteristics {
      .caracteristics {
        gap: 10px 10px;
        grid-template-columns: 1fr 1fr 1fr;

        & > div {
          .title {
            font-size: 14px;
          }

          .content {
            font-size: 14px;
          }
        }
      }
    }

    .accessories {
      flex-wrap: wrap;
    }
  }
}
</style>
