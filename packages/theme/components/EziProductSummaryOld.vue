<template>
  <div class="ezi-product-summary">
    <div class="product-description" v-if="!!product">
      <h1>{{ productLocaleContent.titre_du_produit_pour_le_site }}</h1>
      <h2>{{ productLocaleContent.sous_titre_du_produit_pour_le_site }}</h2>

      <p class="description-content">
        {{ productLocaleContent.phrase_daccroche }}
      </p>

      <p class="price">
        {{
          (product.price_before_discount || product.price_ttx || 0) | currency
        }}
      </p>

      <p class="price-details">{{ $t("price_with_eco_participation") }}</p>

      <p class="delivery">{{ $t("delivery_time") }}</p>

      <div class="product-buttons">
        <!--<button type="submit" class="add-to-cart">{{ $t('add_to_cart') }}</button>-->
        <slot name="addToCartButton"></slot><br /><br />
        <a href="#" class="to-accessories">{{ $t("accessories") }}</a>
      </div>
    </div>
    <!-- <div class="product-image" v-if="productImage">
      <img :src="productImage" alt="" />
    </div> -->
    <div class="product-carousel">
      <EziSlider
        class="main"
        ref="main"
        :options="{
          initialSlide: sortedSlides.length - 1,
        }"
        :as-nav-for="asNavFor1"
      >
        <div
          class="slide"
          v-for="(slide, index) in sortedSlides"
          :key="index"
          :class="`slide--${index}`"
        >
          <img v-if="slide.type == 'image'" :src="slide.link" />
          <div class="product-video" v-else-if="slide.type == 'video'">
            <div v-if="playVideo">
              <video autoplay :muted="isVideoMuted" loop :src="slide.link" />

              <div class="video-sound" @click="isVideoMuted = !isVideoMuted">
                <svg
                  v-if="!isVideoMuted"
                  xmlns="http://www.w3.org/2000/svg"
                  width="40"
                  height="35"
                  viewBox="0 0 40 35"
                >
                  <path
                    id="Icon_awesome-volume-up"
                    data-name="Icon awesome-volume-up"
                    d="M15.119,5,8.864,11.25H1.688A1.687,1.687,0,0,0,0,12.938V23.063A1.687,1.687,0,0,0,1.688,24.75H8.864L15.119,31A1.688,1.688,0,0,0,18,29.811V6.189A1.689,1.689,0,0,0,15.119,5ZM31.525,1.4a1.7,1.7,0,1,0-1.868,2.845,16.428,16.428,0,0,1,0,27.5A1.7,1.7,0,1,0,31.525,34.6a19.832,19.832,0,0,0,0-33.192ZM33.75,18A12.982,12.982,0,0,0,27.719,7.014a1.68,1.68,0,0,0-2.329.525,1.708,1.708,0,0,0,.521,2.346,9.61,9.61,0,0,1,0,16.231,1.707,1.707,0,0,0-.521,2.346,1.683,1.683,0,0,0,2.329.525A12.981,12.981,0,0,0,33.75,18Zm-9.968-5.4a1.688,1.688,0,0,0-1.628,2.957,2.771,2.771,0,0,1,0,4.9A1.688,1.688,0,0,0,23.782,23.4a6.148,6.148,0,0,0,0-10.81Z"
                    transform="translate(0 -1.125)"
                  ></path>
                </svg>
                <svg
                  v-else-if="isVideoMuted"
                  xmlns="http://www.w3.org/2000/svg"
                  width="40"
                  height="35"
                  viewBox="0 0 40 35"
                >
                  <path
                    id="Icon_awesome-volume-mute"
                    data-name="Icon awesome-volume-mute"
                    d="M15.119,5,8.864,11.25H1.688A1.687,1.687,0,0,0,0,12.938V23.063A1.687,1.687,0,0,0,1.688,24.75H8.864L15.119,31A1.688,1.688,0,0,0,18,29.811V6.189A1.689,1.689,0,0,0,15.119,5Zm17.34,13,3.209-3.209a1.135,1.135,0,0,0,0-1.6l-1.6-1.6a1.135,1.135,0,0,0-1.6,0L29.25,14.791l-3.209-3.209a1.135,1.135,0,0,0-1.6,0l-1.6,1.6a1.135,1.135,0,0,0,0,1.6L26.041,18l-3.208,3.208a1.135,1.135,0,0,0,0,1.6l1.6,1.6a1.135,1.135,0,0,0,1.6,0l3.208-3.208,3.209,3.209a1.135,1.135,0,0,0,1.6,0l1.6-1.6a1.135,1.135,0,0,0,0-1.6Z"
                    transform="translate(0 -4.5)"
                  ></path>
                </svg>
              </div>
            </div>

            <div class="play-video" v-else>
              <div class="video">
                <img
                  src="https://eziclean.com/wp-content/uploads/2021/03/CYCLOMAX-R210-MAIN-1-scaled.jpg"
                />
                <div class="play" @click="playVideo = true">
                  <img src="/productpage/playbutton.png" />
                </div>
              </div>
            </div>
          </div>
        </div>
        <template slot="prevButton"
          ><div class="prev-button" @click="playVideo = false"></div
        ></template>
        <template slot="nextButton"
          ><div class="next-button" @click="playVideo = false"></div
        ></template>
      </EziSlider>
      <EziSlider
        class="thumbnails"
        ref="thumbnails"
        :options="{
          navButtons: false,
          slidesToShow: slides.length,
          infinite: false,
          unagile: true,
        }"
        :as-nav-for="asNavFor2"
      >
        <div
          class="slide slide--thumbnail"
          v-for="(slide, index) in sortedSlides"
          :key="index"
          :class="`slide--${index}`"
          @click="
            () => {
              $refs.main.goTo(index);
              playVideo = false;
            }
          "
        >
          <img v-if="slide.type == 'image'" :src="slide.link" />
          <video v-else-if="slide.type == 'video'" :src="slide.link" />
        </div>
      </EziSlider>
    </div>
  </div>
</template>

<script>
import { getFile } from "~/utils/directus/DirectusService";
import EziSlider from "~/components/EziSlider/EziSlider.vue";
//import { productGetters } from '@wiboo/ezishop';

export default {
  components: {
    EziSlider,
  },
  props: {
    product: {
      required: true,
    },
    productLocaleContent: {
      required: true,
    },
  },
  data() {
    return {
      productImage:
        process.env.DIRECTUS_API_URL +
        "/assets/" +
        this.product.featured_image.filename_disk,
      slides: [
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1453831362806-3d5577f014a4?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1496412705862-e0088f16f791?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1506354666786-959d6d497f1a?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1455619452474-d2be8b1e70cd?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
        {
          type: "video",
          link: "/temp/demo.mp4",
        },
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1504674900247-0877df9cc836?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1472926373053-51b220987527?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
        {
          type: "image",
          link: "https://images.unsplash.com/photo-1497534547324-0ebb3f052e88?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb&w=1600&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ",
        },
      ],
      asNavFor1: [],
      asNavFor2: [],
      playVideo: false,
      isVideoMuted: true,
    };
  },
  methods: {
    async getImage(id) {
      return await getFile(id);
    },
  },
  computed: {
    sortedSlides() {
      const sortedSlides = this.slides;

      sortedSlides.sort((a, b) => {
        if (b.type == "video") return -1;
        return 0;
      });

      return sortedSlides;
    },
  },
  mounted() {
    this.asNavFor1.push(this.$refs.thumbnails);
    this.asNavFor2.push(this.$refs.main);
  },
};
</script>

<style lang="scss" scoped>
.product-carousel {
  font-family: "Lato", sans-serif;
  font-weight: 300;
  margin: 0 auto;
  max-width: 100%;
  padding-left: 10%;

  .main {
    margin-bottom: 30px;

    .slide {
      img {
        object-fit: contain;
      }
    }
  }

  .thumbnails {
    margin: 0 -5px;
    width: calc(100% + 10px);

    &.agile--disabled::v-deep .agile__slides {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;

      .slide {
        max-width: 15%;
        height: 50%;
        margin-bottom: 10px;
      }
    }
  }

  .slide {
    align-items: center;
    box-sizing: border-box;
    color: #fff;
    display: flex;
    height: 450px;
    justify-content: center;
  }

  .slide--thumbnail {
    cursor: pointer;
    height: 100px;
    padding: 0 5px;
    transition: opacity 0.3s;
  }

  .slide--thumbnail:hover {
    opacity: 0.75;
  }
  .slide img,
  .slide video {
    height: 100%;
    object-fit: contain;
    object-position: center;
    width: 100%;
  }

  .prev-button,
  .next-button {
    z-index: 1;
    position: absolute;
    font-size: 0;
    background-color: #fff;
    top: 50%;
    box-shadow: 0 1px 12px #0000003b;
    width: 30px;
    height: 85px;
    padding: 0 12px;
    display: block;
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
    background-image: url(/icons/slider/slider-button.png);
  }

  .prev-button {
    transform: rotate(180deg) translateY(50%);
    left: 5%;
  }

  .next-button {
    right: 5%;
    transform: translateY(-50%);
  }

  .product-video {
    position: relative;
    height: 100%;
    display: flex;
    align-content: center;

    .video-sound {
      cursor: pointer;
      position: absolute;
      right: 5%;
      bottom: 15%;
    }

    .play-video {
      height: 100%;

      .video {
        height: 100%;

        .play {
          height: 120px;
          width: 120px;
          position: absolute;
          text-align: center;
          cursor: pointer;
          border-radius: 5px;
          margin: -60px 0 0 -60px;
          left: 50%;
          top: 50%;
          display: block;
          transition: background-color 0.5s ease;
          color: #fff;
        }
      }
    }
  }
}

.ezi-product-summary {
  display: grid;
  grid-template-columns: 40% 60%;

  .product-description {
    h1 {
      font-size: 26px;
    }

    h2 {
      font-weight: 400;
    }

    .description-content {
      margin: 0.5em 0;
      color: #000;
      font-size: 18px;
    }

    .price {
      font-size: 32px;
      font-weight: 600;
      margin-bottom: 30px;
    }

    .price-details {
      font-size: 12px;
      color: grey;
      margin-bottom: 30px;
    }

    .delivery {
      margin-bottom: 30px;
    }

    .product-buttons {
      .add-to-cart {
        background: #118ab2;
        padding: 1em;
        color: #fff;
        margin: 1em 1em 1em 0;
        font-size: 18px;
        border-radius: 3px;
      }

      .to-accessories {
        background-color: #0a0a0a;
        padding: 1em;
        border-radius: 5px;
        color: #fff;
        font-size: 18px;
      }
    }
  }

  .product-image {
    img {
      width: 100%;
    }
  }
}

@media screen and (max-width: 900px) {
  .ezi-product-summary {
    display: block;
  }

  .product-carousel {
    padding-left: 0;
    margin-top: 50px;
  }
}
</style>