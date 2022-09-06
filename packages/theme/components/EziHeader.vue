<template>
  <div class="ezi-header">
    <SfHeader
      class="sf-header--has-mobile-search"
      :class="{
        'show-menu': !scrollToBottom,
      }"
      ref="header"
      wishlistIcon=""
      accountIcon=""
      @click:cart="$nuxt.$emit('toggleCartMenu')"
    >
      <template #logo>
        <nuxt-link :to="localePath('/')" class="sf-header__logo">
          <SfImage
            src="/theme-icons/logo.png"
            alt="EZIClean Icon"
            class="sf-header__logo-image"
          />
        </nuxt-link>
      </template>
      <template #navigation>
        <EziHeaderNavigation :isMenuOpen="isMenuOpen" />
      </template>
      <template #header-icons>
        <div v-e2e="'header-icons'" class="sf-header__icons">
          <SfButton
            class="sf-button--pure sf-header__action"
            aria-label="Toggle cart sidebar"
            @click="toggleCartSidebar"
          >
            <SfIcon class="sf-header__icon" icon="empty_cart" size="1.25rem" />
            <SfBadge
              v-if="totalCartItems"
              class="sf-badge--number cart-badge"
              >{{ totalCartItems }}</SfBadge
            >
          </SfButton>
        </div>
      </template>
      <template #search>
        <SearchBar
          @SearchBar:toggle="isSearchOpen = $event"
          @SearchBar:result="result = $event"
          v-if="!isMobile"
        />
        <div v-else></div>
      </template>
      <template #aside>
        <div class="side-menu">
          <button class="toggle-menu-btn" v-on:click="toggleMenu()">
            <svg
              v-show="!isMenuOpen"
              class="icon-menu-open"
              version="1.1"
              id="icon-open"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              width="24px"
              height="24px"
              viewBox="0 0 24 24"
              enable-background="new 0 0 24 24"
              xml:space="preserve"
              style=""
              fill="grey"
            >
              <rect y="2" width="24" height="2"></rect>
              <rect y="11" width="24" height="2"></rect>
              <rect y="20" width="24" height="2"></rect>
            </svg>
            <svg
              v-show="isMenuOpen"
              class="icon-menu-close"
              version="1.1"
              id="icon-close"
              xmlns="http://www.w3.org/2000/svg"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              x="0px"
              y="0px"
              width="24px"
              height="24px"
              viewBox="0 0 24 24"
              enable-background="new 0 0 24 24"
              xml:space="preserve"
              fill="grey"
            >
              <rect
                x="0"
                y="11"
                transform="matrix(-0.7071 -0.7071 0.7071 -0.7071 12 28.9706)"
                width="24"
                height="2"
              ></rect>
              <rect
                x="0"
                y="11"
                transform="matrix(-0.7071 0.7071 -0.7071 -0.7071 28.9706 12)"
                width="24"
                height="2"
              ></rect>
            </svg>
          </button>
        </div>
      </template>
    </SfHeader>
    <SearchResults
      v-if="isSearchOpen"
      :visible="isSearchOpen"
      :result="result"
    />
    <SfOverlay :visible="isSearchOpen" />
  </div>
</template>

<script>
import {
  SfHeader,
  SfImage,
  SfIcon,
  SfButton,
  SfBadge,
  SfSearchBar,
  SfModal,
  SfOverlay,
  SfMenuItem,
  SfLink,
} from "@storefront-ui/vue";
import { useUiState } from "~/composables";
import { computed, ref } from "@nuxtjs/composition-api";
import EziHeaderNavigation from "~/components/EziHeaderNavigation";
import SearchBar from "~/components/Search/SearchBar";
import SearchResults from "~/components/Search/SearchResults";

export default {
  components: {
    SfHeader,
    SfImage,
    SfModal,
    SfIcon,
    SfButton,
    SfBadge,
    SfOverlay,
    SfMenuItem,
    SfLink,
    EziHeaderNavigation,
    SearchBar,
    SearchResults,
  },
  setup() {
    const { toggleCartSidebar, isMobileMenuOpen } = useUiState();

    const isSearchOpen = ref(false);
    const result = ref(null);

    console.log('EziHeader setup')

    return {
      isSearchOpen,
      result,
      toggleCartSidebar,
      isMobileMenuOpen
    };
  },
  props: {
    totalCartItems: {
      default: 0,
    },
  },
  data() {
    return {
      isMenuOpen: false,
      searchTerms: "",
      isActiveSearchBar: false
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    toggleSearchBar() {
      this.isActiveSearchBar = !this.isActiveSearchBar;
    },
    removeSearchBarText() {
      this.searchTerms = "";
    }
  },
  computed: {
    scrollToBottom() {
      return this.$store.state.scrollDirection.toBottom;
    },
    isMobile() {
      return this.$store.state.isMobile;
    },
  },
  mounted() {
    this.$nuxt.$on("toggleMenu", () => {
      this.toggleMenu();
    });
  },
};
</script>

<style lang="scss" scoped>
.sf-header {
  --header-flex-wrap: nowrap;
  position: fixed;
  z-index: 100;
  width: 100vw;
  transform: translateY(-100px);
  transition: transform 0.3s ease;

  &.show-menu {
    transform: translateY(0px);
  }

  ::v-deep .sf-menu-item {
    --menu-item-label-color: grey;

    span {
      transition: color 0.3s ease;
    }

    :hover {
      --menu-item-label-color: black;
    }
  }

  ::v-deep &__header {
    --header-width: 1200px;
  }

  &__logo-image ::v-deep img {
    height: 80px;
    max-height: 240px;
    max-width: 240px;
    width: 171px;
    position: relative;
    left: 15px;
  }

  ::v-deep &__navigation {
    margin: 0;
    display: flex;
    gap: 20px;

    &.desktop {
      margin: 0 15px 0 30px;
    }

    &.mobile {
      position: fixed;
      top: 100px;
      background-color: white;
      left: 100vw;
      transform: translateX(0%);
      height: 100vh;
      overflow: scroll;
      display: block;
      transition: transform 0.5s ease;
    }
  }

  ::v-deep &__actions {
    justify-content: space-around;
  }

  ::v-deep .sf-header-navigation-item {
    --header-navigation-item-color: grey;
    border: none;
    width: 100%;

    @media screen and (min-width: 1281px) {
      &__item--desktop:not(.dropdown) {
        margin-top: 8px;
      }
    }

    :hover {
      --header-navigation-item-color: black;
    }

    .sf-link {
      border: none;
      font-size: 15px;
      width: max-content;
    }
  }
}

.side-menu {
  display: none;

  & .toggle-menu-btn {
    position: absolute;
    right: 30px;
    top: 50%;
    transform: translateY(-50%);
  }
}

@media screen and (max-width: 1280px) {
  .desktop .sf-header-navigation-item {
    ::v-deep &__link {
      display: none;
    }
  }

  .sf-header::v-deep {
    .sf-header__icons {
      --header-icons-display: none;
    }
  }

  .side-menu {
    display: block;
  }

  .ezi-header {
    /* FIX WRONG STYLE FOR MOBILE */
    &::v-deep .sf-header-navigation-item__item {
      display: block;
      width: 100%;
      padding: 0;
      border: none;

      .sf-link {
        padding: 16px;
        color: rgba(0, 0, 0, 0.4);
        height: var(--button-height);
        display: block;
        width: 100%;
        border-bottom: solid 1px rgb(241, 242, 243);
        font-weight: 500;
        line-height: 22.4px;
      }
    }
  }
}
</style>
