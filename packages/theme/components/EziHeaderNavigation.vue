<template>
  <div>
    <div
      :class="{
        desktop: !isMobile,
        mobile: isMobile,
        'sf-header__navigation': true,
        'menu-open': isMenuOpen,
      }"
    >
      <SfHeaderNavigationItem
        v-for="(menuItem, index) in menuItems"
        :key="index"
        :class="`nav-item ${menuItem.permalien}`"
        v-e2e="`app-header-url_${menuItem.permalien}`"
        :label="menuItem.label"
        :to="
          (menuItem.permalien == 'nos-produits' || menuItem.permalien == 'our-products')
            ? '#'
            : menuItem.permalien
        "
        @mouseenter="EziMegamenuMethod(menuItem.permalien)"
      >
        <template #mobile-navigation-item>
          <div
            class="
              sf-header-navigation-item__item
              sf-header-navigation-item__item--desktop
            "
          >
            <a
              @click="toggleMobileMegamenu"
              :href="index === 0 ? '#' : menuItem.permalien"
              :data-megamenu-tab-index="index"
              class="sf-link sf-header-navigation-item__link"
              >{{ menuItem.label }}</a
            >
            <EziMegamenuVue
              id="megamenu-mobile"
              :isMobileMegamenu="true"
              v-if="isMobile && (menuItem.permalien == 'nos-produits' || menuItem.permalien == 'our-products')"
            />
          </div>
        </template>
        <template #desktop-navigation-item>
          <div
            class="
              sf-header-navigation-item__item
              sf-header-navigation-item__item--desktop
            "
            v-if="isMobile"
          >
            <a
              @click="toggleMobileMegamenu"
              :href="index === 0 ? '#' : menuItem.permalien"
              :data-megamenu-tab-index="index"
              class="sf-link sf-header-navigation-item__link"
              >{{ menuItem.label }}</a
            >
            <EziMegamenuVue
              id="megamenu-mobile"
              :isMobileMegamenu="true"
              v-if="isMobile && (menuItem.permalien == 'nos-produits' || menuItem.permalien == 'our-products')"
            />
          </div>
        </template>
      </SfHeaderNavigationItem>
      <EziMegamenuVue
        id="megamenu-desktop"
        :isMobileMegamenu="false"
        v-show="!isMobile"
      />
      <EziHeaderLocale />
    </div>
    <div
      v-on:click="$nuxt.$emit('toggleMenu')"
      :class="{
        'menu-background': true,
        display: isMenuOpen,
        'full-width': true,
      }"
    ></div>
  </div>
</template>

<script>
import { SfMenuItem, SfModal, SfHeaderNavigationItem } from "@storefront-ui/vue";
import { onSSR } from "@vue-storefront/core";
import { useContent } from '@wiboo/ezishop';
import { useUiState } from "~/composables";
import EziMegamenuVue from "./EziMegamenu.vue";
import EziHeaderLocale from "./EziHeaderLocale";

export default {
  name: "EziHeaderNavigation",
  components: {
    SfMenuItem,
    SfModal,
    EziMegamenuVue,
    EziHeaderLocale,
  },
  props: {
    isMenuOpen: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    toggleMobileMegamenu(e) {
      if (e.target.dataset.megamenuTabIndex) {
        e.preventDefault();
        e.target.classList.toggle("megamenu-mobile-active");
      }
    },
    EziMegamenuMethod: function(slug){
      if (slug == 'nos-produits' || slug == 'our-products') {
        this.$root.$refs.EziMegamenu.getAllProducts();
      }
    },
  },
  setup() {
    const { isMobileMenuOpen, toggleMobileMenu } = useUiState();

    const {
      getMainMenu,
      resMainMenu
    } = useContent();

    onSSR(async () => {
      await getMainMenu();
      console.log('resMainMenu', resMainMenu)
    });

    return {
      isMobileMenuOpen,
      toggleMobileMenu,
      menuItems: resMainMenu
    };
  },
  computed: {
    isMobile() {
      return this.$store.state.isMobile;
    },
  }
};
</script>

<style lang="scss" scoped>
body .sf-header__navigation.mobile.menu-open {
  transform: translateX(-100%);

  & + .menu-background.display {
    opacity: 0.5;
    pointer-events: initial;
  }
}

.menu-background {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.5s ease;
  position: fixed;
  height: calc(100vh + 100px);
  top: 0vh;
  left: 0vw;
  z-index: -1;
  background-color: rgb(104, 104, 104);
}

.sf-header-navigation-item {
  --menu-item-mobile-nav-icon-display: none;
  --menu-item-count-margin: 0;
  width: max-content;
  justify-content: center;

  ::v-deep &__link {
    font-size: 12px;
    padding: 0;
    width: 170px;
    transition: box-shadow 0.3s ease;
  }

  ::v-deep &__item--mobile {
    display: block;
  }
}

.megamenu--desktop {
  opacity: 0;
  visibility: hidden;
  transition: 0.6s ease-in;
  transition-property: opacity, visibility;

  &:hover {
    opacity: 1;
    visibility: visible;
  }
}

.sf-header .mobile .nos-produits,
.sf-header .mobile .our-products {
  border-top: 1px solid rgba(0, 0, 0, 0.12);
}

.megamenu--mobile {
  max-height: 0px;
  transition: max-height 1s ease;
  overflow: hidden;
  border: none;
}

.megamenu-mobile-active {
  box-shadow: inset 0 0 12px rgb(0 0 0 / 12%);
}

.megamenu-mobile-active + .megamenu--mobile {
  max-height: 5000px;
}

.nav-item.nos-produits {
  &:hover ~ .megamenu--desktop {
    opacity: 1;
    visibility: visible;
  }
}

.nav-item.our-products {
  &:hover ~ .megamenu--desktop {
    opacity: 1;
    visibility: visible;
  }
}
</style>
