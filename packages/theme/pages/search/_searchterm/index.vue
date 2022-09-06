<template>
    <div id="searchresults" class="page-margin" v-if="searchterm && results">
        <EziCardsRow :items="products" itemKey="produit" />
    </div>
</template>
<script>
import { onSSR } from "@vue-storefront/core";
import { SfCarousel, SfAddToCart, SfButton } from "@storefront-ui/vue";
import {
  ref,
  app,
  computed,
  useRoute,
  useMeta,
  useContext,
} from "@nuxtjs/composition-api";
import { useProduct, useCart, productGetters } from '@wiboo/ezishop';
import EziCardsRow from "~/components/EziCardsRow";

export default {
  name: "Product",
  components: {
    EziCardsRow,
  },
  setup(app) {
    const { app: { i18n }, redirect } = useContext() ;
    const locale = i18n.locales.find(({ iso, code }) => [iso, code].includes(i18n.locale)) || {};
    const route = useRoute();
    const { products, search } = useProduct('products');
    const searchterm = computed(() => route.value.params.searchterm);
    onSSR(async () => {
      await search({ searchterm: searchterm.value });
    });
    console.log('products',products);
    
    return {
      locale,
      products,
      productGetters
    };
  },
};
</script>
<style lang="scss" scoped>
</style>
