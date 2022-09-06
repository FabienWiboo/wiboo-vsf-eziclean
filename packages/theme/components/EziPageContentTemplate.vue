<template>
  <div :id="pageId" class="ezi-banner full-width">
    <EziBanner
      :video="banner.video"
      :link="banner.link"
      :image="banner.image"
      :alt="banner.alt"
    />

    <EziBreadcrumb />

    <EziTitle :title="title" />

    <EziSubtitle :title="subtitle" />

    <slot name="beforeAccessories"></slot>

    <EziAccessoriesRow v-show="!hideAccessoriesRow" />

    <slot name="beforeServices"></slot>

    <EziServicesRow v-show="!hideServicesRow" />

    <slot name="beforeFooter"></slot>
  </div>
</template>

<script>
import EziAccessoriesRow from "~/components/EziAccessoriesRow";
import EziBanner from "~/components/EziBanner";
import EziBreadcrumb from "~/components/EziBreadcrumb";
import EziServicesRow from "~/components/EziServicesRow";
import EziSubtitle from "~/components/EziSubtitle";
import EziTitle from "~/components/EziTitle";

export default {
  components: {
    EziAccessoriesRow,
    EziBanner,
    EziBreadcrumb,
    EziServicesRow,
    EziSubtitle,
    EziTitle,
  },
  props: {
    pageId: {
      type: String,
      required: true,
      default: "page",
    },
    banner: {
      type: Object,
      required: false,
      default: () => ({
        video: "",
        link: "",
        image: "",
        alt: "",
      }),
    },
    title: {
      type: String,
      default: 'Veuillez insérer un titre {prop: "title"}',
      required: true,
    },
    subtitle: {
      type: String,
      default: 'Veuillez insérer un titre {prop: "subtitle"}',
      required: true,
    },
    hideAccessoriesRow: {
      type: Boolean,
      default: false,
    },
    hideServicesRow: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" scoped>
</style>
