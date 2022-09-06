<template>
  <div class="ezi-breadcrumb">
    <p>
      <a href="/">{{ $t('Home') }}</a>
      <span v-for="(path) in paths" :key="path.link"> > <a :href="path.link">{{ path.title }}</a></span>
    </p>
  </div>
</template>

<script>
export default {
  props: {
    paths: {
      type: Array,
    },
  },
};
</script>

<style lang="scss" scoped>
.ezi-breadcrumb {
  text-align: left;
  margin: 40px 0;

  span {
    color: var(--ezi-primary-color);
  }
}
</style>
