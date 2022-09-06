<template>
    <div class="megamenu--desktop" v-if="!isMobileMegamenu && menuData">
      <div class="megamenu_content">
        <ul class="categories">
          <li
            class="category"
            v-for="category in menuData"
            @mouseenter="displayCategoryDesktop(category)"
            :data-category-id="category.id"
            :key="category.id"
            :class="{
              active: displayedCategoryDesktop === category.id,
            }"
          >
            <div class="category_infos">
              <img
                :src="assetPrefix + category.icon_image.filename_disk"
                width="50"
                height="50"
                :alt="category.name"
                v-if="category.icon_image"
              />
              <h4>{{ category.name }}</h4>
              <svg class="arrow-right" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 256 512"><path d="M64 448c-8.188 0-16.38-3.125-22.62-9.375c-12.5-12.5-12.5-32.75 0-45.25L178.8 256L41.38 118.6c-12.5-12.5-12.5-32.75 0-45.25s32.75-12.5 45.25 0l160 160c12.5 12.5 12.5 32.75 0 45.25l-160 160C80.38 444.9 72.19 448 64 448z"/></svg>
            </div>
            <template v-if="category.subcategories.length>0">
              <div class="seriescontainer">
                <ul class="series">
                  <li
                    class="serie"
                    @mouseenter="displaySerieDesktop(category)"
                    :data-serie-id="category.id"
                    :key="category.id"
                    :class="{
                      active: displayedSerieDesktop.id === category.id,
                    }"
                  >
                    <img
                      :src="assetPrefix + category.thumbnail_listing.filename_disk"
                      width="1000"
                      height="1000"
                      :alt="category.name"
                      v-if="category.thumbnail_listing"
                    />
                    <h2 :inner-html.prop="category.name | serieBoldFormat"></h2>
                  </li>
                  <li
                    class="serie"
                    v-for="serie in category.subcategories"
                    @mouseenter="displaySerieDesktop(serie)"
                    :data-serie-id="serie.id"
                    :key="serie.id"
                    :class="{
                      active: displayedSerieDesktop.id === serie.id,
                    }"
                  >
                    <img
                      :src="assetPrefix + serie.thumbnail_listing.filename_disk"
                      width="1000"
                      height="1000"
                      :alt="serie.name"
                      v-if="serie.thumbnail_listing"
                    />
                    <h2 :inner-html.prop="serie.name | serieBoldFormat"></h2>
                  </li>
                </ul>
                <p class="horizontalbar"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M262.9 67.95C261.5 65.5 258.8 64 256 64C253.2 64 250.5 65.5 249.1 67.95L221.5 51.75C228.7 39.52 241.8 32 256 32C270.2 32 283.3 39.52 290.5 51.75L506.5 419.8C513.8 432.1 513.8 447.4 506.7 459.9C499.6 472.3 486.3 480 472 480H40C25.66 480 12.41 472.3 5.287 459.9C-1.839 447.4-1.756 432.1 5.503 419.8L221.5 51.75L249.1 67.95L33.1 435.1C31.65 438.4 31.63 441.5 33.06 443.1C34.48 446.5 37.13 448 40 448H472C474.9 448 477.5 446.5 478.9 443.1C480.4 441.5 480.4 438.4 478.9 435.1L262.9 67.95z"/></svg></p>
                <template>
                  <ul class="serieproducts"
                    :data-serie-id="category.id"
                    :key="category.id"
                    v-show="displayedSerieDesktop.id == category.id"
                    :class="{
                      active: displayedSerieDesktop.id === category.id,
                    }"
                    >
                    <li
                      class="product"
                      v-for="product in category.products"
                      :key="product.id"
                      data-test
                    >
                      <a :href="'/product/'+product.Product_id.permalien">
                        <img
                          :src="assetPrefix + product.Product_id.featured_image.filename_disk"
                          width="1000"
                          height="1000"
                          :alt="product.Product_id.title"
                          v-if="product.Product_id.featured_image"
                        />
                      </a>
                      <div class="product-text">
                        <h3 v-html="product.Product_id.type"></h3>
                        <p v-html="product.Product_id.title"></p>
                      </div>
                      <div class="pictos">
                        <template
                        v-for="picto in product.Product_id.pictos"
                        >
                          <img :key="picto.id" :src="picto.image">
                        </template>
                      </div>
                      <h3 v-html="product.Product_id.price"></h3>
                      <template v-if="product.inStock">
                        <a class="button"><button v-html="$t('buy_now')"></button></a>
                      </template>
                      <template v-else-if="!product.Product_id.inStock && product.Product_id.preOrder">
                        <a class="button"><button v-html="$t('pre_order')"></button></a>
                      </template>
                      <template v-else>
                        <a class="button" style="visibility:hidden"><button>#</button></a>
                      </template>
                      <a :href="'/product/'+product.Product_id.permalien" class="button" v-html="$t('discover_product')"></a>
                    </li>
                  </ul>
                </template>
                <template
                v-for="serie in category.subcategories"
                >
                  <ul class="serieproducts"
                    :data-serie-id="serie.id"
                    :key="serie.id"
                    v-show="displayedSerieDesktop.id == serie.id"
                    :class="{
                      active: displayedSerieDesktop.id === serie.id,
                    }"
                    >
                    <li
                      class="product"
                      v-for="product in serie.products"
                      :key="product.id"
                      data-test
                    >
                      <a :href="'/product/'+product.Product_id.permalien">
                        <img
                          :src="assetPrefix + product.Product_id.featured_image.filename_disk"
                          width="1000"
                          height="1000"
                          :alt="product.Product_id.title"
                          v-if="product.Product_id.featured_image"
                        />
                      </a>
                      <div class="product-text">
                        <h3 v-html="product.Product_id.type"></h3>
                        <p v-html="product.Product_id.title"></p>
                      </div>
                      <div class="pictos">
                        <template
                        v-for="picto in product.Product_id.pictos"
                        >
                          <img :key="picto.id" :src="picto.image">
                        </template>
                      </div>
                      <h3 v-html="product.Product_id.price"></h3>
                      <template v-if="product.inStock">
                        <a class="button"><button v-html="$t('buy_now')"></button></a>
                      </template>
                      <template v-else-if="!product.Product_id.inStock && product.Product_id.preOrder">
                        <a class="button"><button v-html="$t('pre_order')"></button></a>
                      </template>
                      <template v-else>
                        <a class="button" style="visibility:hidden"><button>#</button></a>
                      </template>
                      <a :href="'/product/'+product.Product_id.permalien" class="button" v-html="$t('discover_product')"></a>
                    </li>
                  </ul>
                </template>
              </div>
            </template>
            <template v-else>
              <ul class="products">
                <li
                  class="product"
                  v-for="product in category.products"
                  :key="product.Product_id.id"
                  data-test
                >
                  <a :href="'/product/'+product.Product_id.permalien">
                    <img
                      :src="assetPrefix + product.Product_id.featured_image.filename_disk"
                      width="1000"
                      height="1000"
                      :alt="product.Product_id.title"
                      v-if="product.Product_id.featured_image"
                    />
                  </a>
                  <div class="product-text">
                    <h3 v-html="product.Product_id.type"></h3>
                    <p v-html="product.Product_id.title"></p>
                  </div>
                  <div class="pictos">
                    <template
                    v-for="picto in product.Product_id.pictos"
                    >
                      <img :key="picto.id" :src="picto.image">
                    </template>
                  </div>
                  <h3 v-html="product.Product_idprice"></h3>
                  <template v-if="product.Product_id.inStock">
                    <a class="button"><button v-html="$t('buy_now')"></button></a>
                  </template>
                  <template v-else-if="!product.Product_id.inStock && product.Product_id.preOrder">
                    <a class="button"><button v-html="$t('pre_order')"></button></a>
                  </template>
                  <template v-else>
                    <a class="button" style="visibility:hidden"><button>#</button></a>
                  </template>
                  <a :href="'/product/'+product.Product_id.permalien" class="button" v-html="$t('discover_product')"></a>
                </li>
              </ul>
            </template>
          </li>
        </ul>
      </div>
    </div>
    <div class="megamenu--mobile" v-else>
      <div class="megamenu_content">
        <ul class="categories">
          <li
            class="category"
            v-for="category in menuData"
            :key="category.id"
          >
            <div
              class="category_infos"
              @click="toggleCategoryMobile($event)"
              :data-category-id="category.id"
            >
              <img
                :src="assetPrefix + category.icon_image.filename_disk"
                width="40"
                height="40"
                :alt="category.name"
                v-if="category.icon_image"
              />
              <h4>{{ category.name }}</h4>
            </div>
            <template v-if="category.subcategories != null">
              <ul class="series">
                <li
                  class="serie"
                  v-for="serie in category.subcategories"
                  :key="serie.id"
                  v-show="serie.categoryId === category.id"
                >
                  <div class="serie_infos"
                    @click="toggleSerieMobile($event)"
                    :data-serie-id="serie.id"
                  >
                    <img
                      :src="assetPrefix + serie.icon_image.filename_disk"
                      width="1000"
                      height="1000"
                      :alt="serie.name"
                      v-if="serie.icon_image"
                    />
                    <h2>Série <b>{{serie.name}}</b></h2>
                  </div>
                  <ul class="serieproducts"
                    :data-serie-id="serie.id"
                    :key="serie.id"
                    >
                    <li
                      class="product"
                      v-for="product in serie.products"
                      :key="product.Product_id.id"
                      data-test
                      v-show="product.Product_id.serieId === serie.id"
                    >
                      <a :href="'/product/'+product.Product_id.permalien">
                        <img
                          :src="assetPrefix + product.Product_id.featured_image.filename_disk"
                          width="1000"
                          height="1000"
                          :alt="product.Product_id.title"
                          v-if="product.Product_id.featured_image"
                        />
                      </a>
                      <div class="product-text">
                        <h3 v-html="product.Product_id.type"></h3>
                        <p v-html="product.Product_id.title"></p>
                      </div>
                      <div class="pictos">
                        <template
                        v-for="picto in product.Product_id.pictos"
                        >
                          <img :key="picto.id" :src="picto.image">
                        </template>
                      </div>
                      <h3 v-html="product.Product_id.price"></h3>
                      <template v-if="product.Product_id.inStock">
                        <a class="button"><button v-html="$t('buy_now')"></button></a>
                      </template>
                      <template v-else-if="!product.Product_id.inStock && product.Product_id.preOrder">
                        <a class="button"><button v-html="$t('pre_order')"></button></a>
                      </template>
                    </li>
                  </ul>
                </li>
              </ul>
            </template>
            <template v-else>
              <ul class="products">
                <li
                  class="product"
                  v-for="product in category.products"
                  :key="product.Product_id.id"
                  data-test
                  v-show="product.Category_id.id === category.id"
                >
                  <a :href="'/product/'+product.Product_id.permalien">
                    <img
                      :src="assetPrefix + product.Product_id.featured_image.filename_disk"
                      width="1000"
                      height="1000"
                      :alt="product.Product_id.title"
                      v-if="product.Product_id.featured_image"
                    />
                  </a>
                  <div class="product-text">
                    <h3 v-html="product.Product_id.type"></h3>
                    <p v-html="product.Product_id.title"></p>
                  </div>
                  <div class="pictos">
                    <template
                    v-for="picto in product.Product_id.pictos"
                    >
                      <img :key="picto.id" :src="picto.image">
                    </template>
                  </div>
                  <h3 v-html="product.Product_id.price"></h3>
                  <template v-if="product.Product_id.inStock">
                    <a class="button"><button v-html="$t('buy_now')"></button></a>
                  </template>
                  <template v-else-if="!product.Product_id.inStock">
                    <a class="button"><button v-html="$t('pre_order')"></button></a>
                  </template>
                </li>
              </ul>
            </template>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
//import menuDataJson from "/assets/menutest.json";
import menuData from "/assets/menutest.json";
import { onSSR } from "@vue-storefront/core";
import { useContent } from '@wiboo/ezishop';
import { computed } from '@nuxtjs/composition-api';

export default {
  setup() {

    //console.log('menuDataJson',menuDataJson);

    const {
      getMegamenu,
      resMegamenu
    } = useContent();

    onSSR(async () => {
      await getMegamenu();
    });

    //console.log('resMegamenu',resMegamenu.value)
    

    return {
      menuData: resMegamenu,
      
      /*
      displayedCategoryDesktop: computed(() => resMegamenu.value[0].id ),
      displayedSerieDesktop: computed(() => resMegamenu.value[0].subcategories[0] ),
      /*
      opennedCategory: null,
      opennedCategoryId: null,
      opennedCategoryHeight: 0,
      opennedSerie: null,
      opennedSerieId: null,
      */
    };
    

  },
  
  data() {
    return {
      assetPrefix: process.env.DIRECTUS_API_URL + "/assets/",
      //menuData: menuData,
      displayedCategoryDesktop: 1299,
      displayedSerieDesktop:  10615,
      opennedCategory: null,
      opennedCategoryId: null,
      opennedCategoryHeight: 0,
      opennedSerie: null,
      opennedSerieId: null,
    };
  },
  
  props: {
    isMobileMegamenu: {
      default: false,
      type: Boolean,
    },
  },
  methods: {
    displayCategoryDesktop(category) {
      this.displayedCategoryDesktop = category.id;
      if (category.subcategories != null) {
        const array = [];
        category.subcategories.forEach(serie => {
          array.push(serie.id)
        });
        if (category.subcategories && category.subcategories[0]) {
          this.displayedSerieDesktop = category.subcategories[0]
        };
      }
      setTimeout(() => {
        this.getAllProducts();
      }, 10)
    },
    displaySerieDesktop(serie) {
      this.displayedSerieDesktop = serie;
      setTimeout(() => {
        this.getAllProducts();
      }, 10)
    },
    toggleCategoryMobile(event) {
      const categoryId = event.currentTarget.dataset.categoryId;
      const category = event.currentTarget.parentNode;
      const sibling = event.currentTarget.nextElementSibling;
      const siblingChildClass = sibling.firstChild.className;
      const querySiblingChilds = category.querySelectorAll(`.${siblingChildClass}`);
      let height = 0;

      // GET HEIGHT IF SERIE OR PRODUCT
      if (siblingChildClass == "serie") {
        querySiblingChilds.forEach(serie => {
          height += 60;
        })
      }
      else if (siblingChildClass == "product") {
        querySiblingChilds.forEach(product => {
          height += product.getBoundingClientRect().height;
          height += 22;
        });
      }
      // HEIGHT TO APPLY TO CATEGORY
      height += "px";

      // IF A SERIE IS OPENNED, CLOSE IT
      if (this.opennedSerie) {
        const serieProducts = this.opennedSerie.querySelector(".serieproducts");
        if (serieProducts.getBoundingClientRect().height != 0) {
          serieProducts.style.height = 0;
        }
      }
      
      // OPENNING CATEGORY
      if (categoryId == this.opennedCategoryId) {
        sibling.style.height = 0;
        this.opennedCategory = null;
        this.opennedCategoryId = null;
      }
      else if (categoryId !== this.opennedCategoryId & this.opennedCategoryId != null) {
        const opennedCategory = this.opennedCategory.querySelector(".category_infos").nextElementSibling
        opennedCategory.style.height = 0;

        this.openMobileCategory(sibling, category, categoryId, height);
      }
      else {
        this.openMobileCategory(sibling, category, categoryId, height);
      }
    },
    openMobileCategory(sibling, category, categoryId, height) {
      this.opennedCategory = category;
      this.opennedCategoryId = categoryId;
      this.opennedCategoryHeight = height;
      sibling.style.height = height;
      setTimeout(function () {
        category.scrollIntoView({behavior: "smooth"});
      }, 300);
    },
    toggleSerieMobile(event) {
      const serieId = event.currentTarget.dataset.serieId;
      const series = event.currentTarget.parentNode.parentNode;
      const serie = event.currentTarget.parentNode;
      const sibling = event.currentTarget.nextElementSibling;
      const siblingChildClass = sibling.firstChild.className;
      const querySiblingChilds = serie.querySelectorAll(`.${siblingChildClass}`);
      let height = 0;

      querySiblingChilds.forEach(product => {
        height += product.getBoundingClientRect().height;
        height += 22;
      });
      let seriesHeight = parseInt(this.opennedCategoryHeight, 10) + height + "px";
      height += "px";
     

      if (serieId == this.opennedSerieId) {
        this.opennedSerie = null;
        this.opennedSerieId = null;
        series.style.height = this.opennedCategoryHeight;
        sibling.style.height = 0;
      }
      else if (serieId !== this.opennedSerieId & this.opennedSerieId != null) {
        const opennedSerie = this.opennedSerie.querySelector(".serie_infos").nextElementSibling
        opennedSerie.style.height = 0;

        this.openMobileSerie(sibling, serie, series, serieId, seriesHeight, height);
      }
      else {
        this.openMobileSerie(sibling, serie, series, serieId, seriesHeight, height);
      }
    },
    openMobileSerie(sibling, serie, series, serieId, seriesHeight, height) {
      this.opennedSerie = serie;
      this.opennedSerieId = serieId;
      series.style.height = seriesHeight;
      sibling.style.height = height;
      setTimeout(function () {
        serie.scrollIntoView({behavior: "smooth"});
      }, 300);
    },

    getAllProducts() {
      const category = document.querySelector(".megamenu--desktop").querySelector(".category.active");
      const serie = category.querySelector(".serieproducts.active");
      if (serie) {
        const serieproductstext = serie.querySelectorAll(".product-text");
        this.setProductsHeight(serieproductstext);
      }
      else {
        const productstext = category.querySelectorAll(".product-text");
        this.setProductsHeight(productstext);
      }
    },
    setProductsHeight(productsText) {
      const height = [];
      productsText.forEach(productText => {
        let divHeight = productText.children[0].getBoundingClientRect().height + productText.children[1].getBoundingClientRect().height
        height.push(divHeight);
      })

      let productTextHeight = Math.max(...height);

      productsText.forEach(productText => {
        if (productText.getBoundingClientRect().height != productTextHeight) {
          productText.style.height = productTextHeight + "px";
        }
      })
    }
  },
  mounted() {
    this.$root.$refs.EziMegamenu = this;
    console.log('menuData',this.menuData)
  },
};
</script>

<style lang="scss" scoped>
li,
ul {
  list-style: none;
}

h2 {
  font-size: 24px;
  line-height: 24px;
  font-weight: 400;
}
h3 {
  font-size: 19px;
  line-height: 19px;
}
h4 {
  font-size: 16px;
}
p {
  font-size: 15px;
}
a {
  cursor: pointer;
  font-size: 15px;
}
.megamenu--mobile {
  display: none;
}
.megamenu--desktop {
  --padding-megamenu-side: 1.5em;
  --padding-megamenu-top: 1em;
  --padding-megamenu-bottom: 2em;
  --gap-between-products: 12px;

  --primary-blue: #0085ad;
  --secondary-blue: #00c0dd;
  --primary-grey: #717171;
  --secondary-grey: #c2c2c2;

  width: 90vw;
  min-width: 50%;
  height: auto;
  background: white;
  left: 50%;
  transform: translateX(-50%);
  box-shadow: 0 0 20px rgb(0 0 0 / 15%);
  position: absolute;
  z-index: 999;
  top: 100px;

  .megamenu_content {
    padding: var(--padding-megamenu-top) calc(var(--padding-megamenu-side) * 2)
      var(--padding-megamenu-bottom);
    position: relative;
  }
  .categories {
    //min-height: fit-content;
    min-height: 70vh;
    width: calc(25% - var(--padding-megamenu-side));
    border-right: 1px solid grey;
  }
  .category {
    padding: 10px 15px;
    height: auto;
    width: 95%;
    margin-top: 1em;

    box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;

    h4 {
      padding-right: 10px;
    }
  }
  .category_infos {
    display: flex;
    align-items: center;
    white-space: initial;

    img {
      width: 30px;
      height: 30px;
      margin-right: 10px;
    }

    h4 {
      flex: 5;
    }

    svg {
      fill: var(--primary-blue);
      height: 12px;
      transition: all 0.25s ease-in-out;
    }
  }
  .category.active {
    outline: 3px solid var(--primary-blue);
    .arrow-right {
      fill: var(--secondary-blue);
    }
    .seriescontainer, .series, .horizontalbar, .serieproducts, .products {
      display: grid;
    }
  }
  .products {
    grid-template-columns: repeat(4, calc(25% - var(--gap-between-products)));
    grid-gap: var(--gap-between-products);
    position: absolute;
    overflow-y: scroll;
    right: var(--padding-megamenu-side);
    top: 0;
    width: calc(75% - var(--padding-megamenu-side));
    padding-left: 20px;
    padding-bottom: var(--padding-megamenu-bottom);
    height: 100%;
  }
  .seriescontainer {
    grid-template-rows: min-content 60px auto;
    position: absolute;
    right: var(--padding-megamenu-side);
    top: 0;
    width: calc(75% - var(--padding-megamenu-side));
    padding-left: 20px;
    padding-bottom: var(--padding-megamenu-bottom);
    height: 100%;
    overflow-y: scroll;
    .product {
      margin-top: unset;
    }
  }
  .series {
    grid-template-columns: repeat(4, calc(25% - var(--gap-between-products)));
    grid-gap: var(--gap-between-products);
  }
  .serie {
    margin-top: var(--padding-megamenu-top);
    box-shadow: rgba(100, 100, 111, 0.2) 0 7px 29px 0;
    height: fit-content;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 20px;

    img {
      width: 8vw;
      height: 8vw;
    }

    &.active {
    background-color: #f3f3f3;
    }
  }
  .horizontalbar {
    grid-template-columns: 47% auto 47%;
    grid-column-gap: 1.2rem;
    width: 100%;
    padding-right: var(--gap-between-products);
    align-items: center;
    justify-content: center;

    &::before, &::after {
      content: "";
      display: block;
      height: 1px;
      background-color: grey;
    }
    svg {
      height: 18px;
      transform: rotate(180deg);
    }
  }
  .serieproducts {
    grid-template-columns: repeat(4, calc(25% - var(--gap-between-products)));
    grid-gap: var(--gap-between-products);
  }
  .product {
    margin-top: var(--padding-megamenu-top);
    box-shadow: rgba(100, 100, 111, 0.2) 0 7px 29px 0;
    height: fit-content;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 15px 20px;

    img {
      width: 8vw;
      height: 8vw;
    }

    .pictos {
      display: grid;
      grid-template-columns: repeat(5, 20%);
      width: 100%;
      padding: 10px 0 6px;

      img {
        height: 100%;
        width: 100%;
      }
    }

    .button {
      height: 36px;
      width: 80%;
      background: var(--primary-blue);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: all 0.25s ease-in-out;
      border-radius: 3px;
      border: 2px solid rgba(0, 0, 0, 0.15);
      margin-top: 10px;

      &:hover {
        background: var(--secondary-blue);
      }
    }

    .button:last-child {
      background: var(--primary-grey);

      &:hover {
        background: var(--secondary-grey);
      }
    }
  }
  .seriescontainer, .series, .horizontalbar, .serieproducts, .products {
    display: none;
  }
}

@media screen and (max-width: 1440px) {
  h2 {
    font-size: 20px;
    line-height: 20px;
  }
  h3 {
    font-size: 16px;
    line-height: 16px;
  }
  h4 {
    font-size: 14px;
  }
  p {
    font-size: 14px;
  }
  a {
    font-size: 14px;
  }
  .megamenu--desktop {
    .categories {
      width: calc(calc(100% / 3) - var(--padding-megamenu-side));
    }
    .products {
      grid-template-columns: repeat(3, calc(calc(100% / 3) - var(--gap-between-products)));
      width: calc(calc(2 * calc(100% / 3)) - var(--padding-megamenu-side));
    }
    .seriescontainer {
      width: calc(calc(2 * calc(100% / 3)) - var(--padding-megamenu-side));
    }
    .series {
      grid-template-columns: repeat(3, calc(calc(100% / 3) - var(--gap-between-products)));
    }
    .serieproducts {
      grid-template-columns: repeat(3, calc(calc(100% / 3) - var(--gap-between-products)));
    }
    .product {
      .pictos {
        padding: 8px 0 4px;
      }
      .button {
        height: 32px;
        margin-top: 8px;
      }
    }
  }
}

@media screen and (max-width: 1280px) {
  .megamenu--desktop {
    display: none;
  }

  .megamenu--mobile {
    display: block;
  }
}

.megamenu--mobile {
  --primary-white: #f4f4f4;
  --secondary-white: #d0d0d0;
  --tertiary-white: ;
  --primary-blue: #0085ad;
  --secondary-blue: #00c0dd;

  padding: 0;
  width: 260px;

  h2 {
    font-size: 18px;
    line-height: 18px;
  }
  h3 {
    font-size: 14px;
    line-height: 14px;
  }
  h4 {
    font-size: 12px;
  }
  p {
    font-size: 12px;
  }
  a {
    font-size: 12px;
  }
  .category_infos {
    background-color: var(--primary-white);
    border: solid var(--secondary-white);
    border-width: 1px 0px;
    display: grid;
    grid-template-columns: 25% 75%;
    color: black;
    padding: 12px 8px;
    align-items: center;

    h4 {
      letter-spacing: 1px;
      text-align: center;
      font-family: "Raleway" !important;
    }
  }

  .series {
    img {
      width: 50px;
      height: 50px;
    }
    svg {
      height: 10px;
    }
  }
  
  .serie_infos {
    border: solid var(--secondary-white);
    border-width: 1px 0px;
    display: grid;
    grid-template-columns: 25% 75%;
    padding: 4px 8px;
    align-items: center;
  }
  
  .products, .series, .serieproducts {
    height: 0;
    transition: height 0.3s ease;
    overflow: hidden;
  }

  .product {
    border-bottom: 1px solid var(--secondary-white);
    height: auto;

    a {
      margin-top: 22px;
      display: block;

      img {
        width: 50%;
        height: auto;
      }
    }

    h2 {
      text-transform: none;
      font-size: 14.5px;
      color: rgba(0, 0, 0, 0.6);
      font-weight: 400;
    }

    h3 {
      color: #000;
      font-size: 18px;
      margin-bottom: 10px;
    }

    .pictos {
      img {
        width: 50px;
        height: 50px;
      }
    }

    .button {
      margin-top: unset;
      margin-bottom: 20px;

      button {
        background-color: var(--primary-blue);
        padding: 12px 20px;
        width: 200px;
        border-radius: 3px;
        border: 2px solid rgba(0, 0, 0, 0.12);
        text-transform: uppercase;
        color: white;
        font-weight: 600;
        font-size: 12px;
        letter-spacing: 1px;

        &:hover {
          background-color: var(--secondary-blue);
        }
      }
    }
  }
}
</style>