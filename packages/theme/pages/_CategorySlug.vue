<template>
  <EziPageContentTemplate
    :banner="( { image : category.banner } || { video: '/categories/videos/cleaner.mp4' } )"
    :title="(category.name || 'Les produits Eziclean')"
    :subtitle="$t('discover_our_products')"
    pageId="category"
    alignBreadcrumb="center"
  >
    <template #beforeAccessories>
      <EziCardsRow :items="category.products" itemKey="produit" />
    </template>
  </EziPageContentTemplate>
</template>

<script>
import { onSSR } from "@vue-storefront/core";
import {
  app,
  computed,
  useRoute,
  useContext,
  useMeta
} from "@nuxtjs/composition-api";
import { useContent } from '@wiboo/ezishop';
import EziCardsRow from "~/components/EziCardsRow";
import EziPageContentTemplate from "~/components/EziPageContentTemplate";

export default {
  components: {
    EziCardsRow,
    EziPageContentTemplate,
  },
  setup() {
    const { app: { i18n }, redirect } = useContext() ;
    const locale = i18n.locales.find(({ iso, code }) => [iso, code].includes(i18n.locale)) || {};
    const route = useRoute();

    const slug = computed(() => route.value.params.CategorySlug);

    const { resCategory, getCategory } = useContent();
    
    onSSR(async () => {
      await getCategory({slug : slug.value});
    });

    return {
      locale,
      category: resCategory,
    };
  },
  head() {
    if(this.category){
      return {
        title: (this.category.meta_title ?? '') + ' | ' + (process.env.META_SHOP_NAME || "Eziclean"),
        meta: [
          {
            hid: 'description',
            name: 'description',
            content: (this.category.meta_description ?? '')
          }
        ],
      }
    }
  },
};
</script>

<style lang="scss" scoped>
.ezi-row-card {
  margin: 50px 12.5vw;
}
</style>
