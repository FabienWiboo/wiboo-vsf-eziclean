<template>
  <SfSearchBar
    v-click-outside="closeSearch"
    :placeholder="$t('Search for items')"
    aria-label="Search"
    class="sf-header__search"
    :value="term"
    @input="handleSearch"
    @keydown.enter="handleSearch($event)"
    @keydown.tab="hideSearch"
    @focus="showSearch"
    @click="showSearch"
    @keydown.esc="closeSearch"
  >
    <template #icon>
        <SfButton
            @click="toggleSearchBar"
            aria-label="Open search"
            class="sf-search-bar__button sf-button--pure"
        >
            <span class="sf-search-bar__icon">
                <SfIcon
                    color="var(--c-text)"
                    size="20px"
                    :icon="isActiveSearchBar ? 'cross' : 'search'"
                    @click="toggleSearch()"
                />
            </span>
        </SfButton>
    </template>
  </SfSearchBar>
</template>

<script>
import { SfButton, SfSearchBar, SfIcon } from '@storefront-ui/vue';
import { defineComponent, ref } from '@nuxtjs/composition-api';
import { clickOutside } from '~/utilities/directives/click-outside/click-outside-directive.js';
import debounce from 'lodash.debounce';
import { useProduct, productGetters } from '@wiboo/ezishop';
import { watch, useRoute } from '@nuxtjs/composition-api';
export default defineComponent({
  name: 'SearchBar',
  components: {
    SfSearchBar,
    SfIcon,
    SfButton,
  },
  directives: { clickOutside },
  props: {
    itemsPerPage: {
      type: Number,
      default: 12,
    },
    minTermLen: {
      type: Number,
      default: 3,
    },
  },
  setup({ itemsPerPage, minTermLen }, { emit }) {
    const term = ref('');
    const isSearchOpen = ref(false);
    const result = ref(null);
    const route = useRoute()
    const { products, search } = useProduct('products');
    const showSearch = () => {
        console.log('showSearch')
        if (!isSearchOpen.value) {
            isSearchOpen.value = true;
            emit('SearchBar:toggle', true);
            if (document) {
                document.getElementsByClassName('sf-search-bar')[0].classList.add('active');
                document.body.classList.add('no-scroll');
            }
        }
    };
    const hideSearch = () => {
        console.log('hideSearch')
        if (isSearchOpen.value) {
            isSearchOpen.value = false;
            emit('SearchBar:toggle', false);
            emit('SearchBar:result', {});
            if (document) {
              term.value = '';
              document.getElementsByClassName('sf-search-bar')[0].classList.remove('active');
              document.body.classList.remove('no-scroll');
            }
        }
    };
    const toggleSearch = () => {
        console.log('toggleSearch')
        if (isSearchOpen.value) {
            hideSearch();
        } else {
            showSearch();
        }
    };
    
    const closeSearch = (event) => {
        console.log('closeSearch')
        if (document) {
            const searchResultsEl = document.getElementsByClassName('search');
            console.log('searchResultsEl',searchResultsEl);
            if (searchResultsEl.length>0 && !searchResultsEl[0].contains(event.target)) {
            hideSearch();
            term.value = '';
            }
        } else {
            hideSearch();
            term.value = '';
        }
    };
    const handleSearch = debounce(async (paramValue) => {
      term.value = !paramValue.target ? paramValue : paramValue.target.value;
      if (term.value.length < minTermLen) return;
      await Promise.all([
        search({
          itemsPerPage,
          term: term.value,
        })
      ]);
      console.log('products.value',products.value);
      result.value = {
        products: products.value
      };
      emit('SearchBar:result', result.value);
    }, 1000);
    watch(route, () => {
      hideSearch();
      term.value = '';
    });
    return {
      closeSearch,
      showSearch,
      hideSearch,
      toggleSearch,
      handleSearch,
      isSearchOpen,
      term,
      isActiveSearchBar: false
    };
  },
  methods: {
    toggleSearchBar() {
        this.isActiveSearchBar = !this.isActiveSearchBar;
    },
    removeSearchBarText() {
      this.term = "";
    }
  }
});
</script>
<style lang="scss" scoped>
.sf-search-bar__button {
  position: relative;
  right: 6px;
  bottom: 0;
}
.sf-search-bar {
  --header-search-flex: 0 0 30%;

  width: max-content;
  background-color: rgba(13, 132, 174);
  padding: 10px 0px;
  flex: unset;

  &__input {
    width: 25px;
    padding:0 5px;
    height:20px;
  }

  &.active::v-deep &__input {
    width: 150px;
    padding:0 5px;
    height:20px;

    &::placeholder {
      color: rgb(255, 255, 255);
    }
  }

  ::v-deep &__input {
    color: white;
    border: none;
    outline: none;
    width: 20px;
    transition: width 0.3s ease;

    &::placeholder {
      color: rgba(13, 132, 174);
      opacity: 0.5;
    }
  }

  & ::v-deep .sf-icon svg {
    --icon-color: white;
    margin-left: -10px;
  }
}
.sf-search-bar__input.extended {
    width:150px !important;
}
</style>