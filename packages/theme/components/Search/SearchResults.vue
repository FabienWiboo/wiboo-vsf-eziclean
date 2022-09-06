<template>
  <div>
    <SfMegaMenu
      :visible="isSearchOpen"
      :title="$t('Search results')"
      class="search"
    >
      <transition
        name="sf-fade"
        mode="out-in"
      >
        <div
          v-if="products && products.length > 0"
          key="results"
          class="search__wrapper-results"
        >
          <SfMegaMenuColumn
            :title="$t('Product suggestions')"
            class="sf-mega-menu-column--pined-content-on-mobile search__results"
          >
            <template #title="{title}">
              <SfMenuItem
                :label="title"
                class="sf-mega-menu-column__header search__header"
              >
                <template #mobile-nav-icon>
                  &#8203;
                </template>
              </SfMenuItem>
            </template>
            <SfScrollable
              class=""
              maxContentHeight="100vh"
              show-text=""
              hide-text=""
            >
              <div class="results-listing">
                <div class="ezi-row-card">
                  <SearchItemCard
                    v-for="(product, i) in products"
                    :key="'searchProduct-'+i"
                    :imageLink="product.img"
                    :price="product.price"
                    :cardLink="product.link"
                  >
                    <template #title>{{ product.title }}</template>
                    <template #content>{{ product.content }}</template>
                  </SearchItemCard>
                </div>
              </div>
            </SfScrollable>
          </SfMegaMenuColumn>
          <div class="action-buttons smartphone-only">
            <SfButton
              class="action-buttons__button color-light"
              @click="$emit('close')"
            >
              {{ $t('Cancel') }}
            </SfButton>
          </div>
        </div>
        <div
          v-else-if="products && products.length == 0"
          key="no-results"
          class="before-results"
        >
          <p class="before-results__paragraph"></p>
          <p class="before-results__paragraph">
            {{ $t('You haven’t searched for items yet') }}
          </p>
          <p class="before-results__paragraph">
            {{ $t('We haven’t found any results for given phrase') }}
          </p>
          <SfButton
            class="before-results__button color-secondary smartphone-only"
            @click="$emit('close')"
          >
            {{ $t('Go back') }}
          </SfButton>
        </div>
        <div
          v-else
          key="no-results"
          class="before-results"
        >
          <p class="before-results__paragraph"></p>
          <p class="before-results__paragraph">
            {{ $t('You haven’t searched for items yet') }}
          </p>
          <p class="before-results__paragraph">
            {{ $t('Let’s start now – we’ll help you') }}
          </p>
          <SfButton
            class="before-results__button color-secondary smartphone-only"
            @click="$emit('close')"
          >
            {{ $t('Go back') }}
          </SfButton>
        </div>
      </transition>
    </SfMegaMenu>
  </div>
</template>
<script>
import {
  SfMegaMenu,
  SfList,
  SfProductCard,
  SfScrollable,
  SfMenuItem,
  SfButton,
} from '@storefront-ui/vue';
import SearchItemCard from "~/components/Search/SearchItemCard";
import {
  ref,
  computed,
  defineComponent,
} from '@nuxtjs/composition-api';
import { productGetters } from '@wiboo/ezishop';
import { useUiHelpers, useImage } from '~/composables';

export default defineComponent({
  name: 'SearchResults',
  components: {
    SfMegaMenu,
    SfList,
    SfProductCard,
    SfScrollable,
    SfMenuItem,
    SfButton,
    SearchItemCard
  },
  props: {
    visible: {
      type: Boolean,
      default: false,
    },
    result: {
      type: Object,
      default: () => ({}),
    }
  },
  setup(props) {

    const th = useUiHelpers();
    const isSearchOpen = ref(props.visible);
    const products = computed(() => props.result?.products);

    return {
      th,
      isSearchOpen,
      productGetters,
      products
    };
  },
});
</script>
<style lang="scss" scoped>
.search {
  position: absolute;
  right: 0;
  left: 0;
  z-index: 51;
  --mega-menu-column-header-margin: var(--spacer-sm) 0 var(--spacer-xl);
  --mega-menu-content-padding: 0;
  //--mega-menu-height: auto;
  @include for-desktop {
    --mega-menu-content-padding: var(--spacer-xl) 0;
  }
  &__wrapper-results {
    display: flex;
    flex-direction: column;
    @include for-desktop {
      flex-direction: row;
      flex: 1;
    }
  }
  &__categories {
    flex: 0 0 220px;
  }
  &__results {
    flex: 1
  }
  &__header {
    margin-left: var(--spacer-sm);
  }
  ::v-deep .sf-bar {
    display: none;
  }
  ::v-deep .sf-mega-menu-column__header {
    display: none;
    @include for-desktop {
      display: flex;
    }
  }
}
.results {
  &--desktop {
    --scrollable-max-height: 35vh;
  }
  &--mobile {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    background: var(--c-white);
    padding: var(--spacer-base) var(--spacer-sm);
    --product-card-max-width: 9rem ;
  }
}
.see-all {
  padding: var(--spacer-xl) 0 0 var(--spacer-sm);
}
.action-buttons {
  padding: var(--spacer-xl) var(--spacer-sm) var(--spacer-3xl);
  background: var(--c-white);
  width: 100%;
  &__button {
    width: calc(100% - 32px);
  }
}
.results-listing {
  display: grid;
  margin-left: var(--spacer-2xs);
  padding: 10px 0;
}
.result-card {
  margin: var(--spacer-sm) 0;
  @include for-desktop {
    margin: var(--spacer-2xs) 0;
  }
}

.simplebar-wrapper,
.simplebar-mask {
  height: 100% !important;
}
.sf-scrollable__content {
  max-height: 100% !important;
}

.before-results {
  box-sizing: border-box;
  padding: var(--spacer-base) var(--spacer-sm) var(--spacer-2xl);
  width: 100%;
  text-align: center;
  @include for-desktop {
    padding: 0;
  }
  &__picture {
    --image-width: 230px;
    margin-top: var(--spacer-2xl);
    @include for-desktop {
      --image-width: 18.75rem;
      margin-top: var(--spacer-base);
    }
  }
  &__paragraph {
    font-family: var(--font-family--primary);
    font-weight: var(--font-weight--normal);
    font-size: var(--font-size--base);
    color: var(--c-text-muted);
    margin: 0;
    @include for-desktop {
      font-size: var(--font-size--lg);
    }
    &:first-of-type {
      margin: var(--spacer-xl) auto var(--spacer-xs);
    }
  }
  &__button {
    margin: var(--spacer-xl) auto;
    width: 100%;
  }
}
.ezi-row-card {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 15px;
}

@media screen and (max-width: 425px) {
  .ezi-row-card.product-style {
    display: grid;
    grid-template-columns: 1fr;
    gap: 15px;
  }
}

@media screen and (min-width: 1440px) {
  .ezi-row-card.product-style {
    grid-template-columns: repeat(4, 1fr);
    gap: 3vw;
  }
}
</style>