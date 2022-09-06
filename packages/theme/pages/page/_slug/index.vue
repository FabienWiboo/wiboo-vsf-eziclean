<template>
  <div v-if="page" class="page page-margin">
      <h1>{{ page.localeContent.title }}</h1>
      <div v-html="page.localeContent.content"></div>
  </div>
</template>
<style lang="scss" scoped>
.page {
  margin-bottom:2rem;
}
</style>
<script>
import { onSSR } from "@vue-storefront/core";
import { useContent } from '@wiboo/ezishop';
import { useContext, useRoute, computed } from "@nuxtjs/composition-api";
export default {
  name: "Page",
  setup() {
      
      const {
          app: { i18n },
          redirect,
      } = useContext();

      const locale =
      i18n.locales.find(({ iso, code }) => [iso, code].includes(i18n.locale)) ||
      {};
      
      const route = useRoute();
      const { resPage, getPage } = useContent();
      const slug = computed(() => route.value.params.slug);

      onSSR(async () => {
          await getPage({ slug: slug.value });
      });

      if(resPage.value && resPage.value && slug.value!=resPage.value.permalien){
          const redirect_path = ((locale.code!="fr") ? '/'+locale.code : '')+'/page/'+resPage.value.permalien;
          redirect( redirect_path );
      }

      return {
          page: resPage
      };

  },
  head() {
  if(this.page){
    return {
      title: (this.page.meta_title ?? '') + ' | ' + (process.env.META_SHOP_NAME || "Eziclean"),
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: (this.page.meta_description ?? '')
        }
      ],
    }
  }
},
};
</script>