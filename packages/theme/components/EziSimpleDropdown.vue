<template>
  <div
    class="dropdown sf-header-navigation-item__item"
    @click="toggleRiskLevels"
    :class="{ 'dropdown-active': isBottomSectionToggled }"
    :style="[
      { '--options-height': optionsHeight + 'px' },
      { '--option-height': optionHeight + 'px' },
      { '--main-el-border-radius': borderRadius },
      { '--dropdown-width': width },
      { '--dropdown-background-color': backgroundColor },
      { '--dropdown-border': border },
      { '--dropdown-hover-background-color': hoverBackgroundColor },
      { '--dropdown-default-text-color': textColor },
    ]"
  >
    <p class="text" v-html="config.placeholder ? config.placeholder : ''"></p>
    <i class="angle-down"></i>
    <div v-show="isBottomSectionToggled" class="options">
      <div
        v-for="option in configOptions"
        :key="option.code"
        class="option"
        @click="setCurrentSelectedOption(option)"
      >
        <img
          :src="'/flags/' + option.code + '.png'"
          data-test
          v-show="option.code"
        />
        <p>&nbsp;{{ option.label }}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "dropdown",
  data() {
    return {
      isBottomSectionToggled: false,
      optionsHeight: 0,
      optionHeight: 35,
      width: "max-content",
      configOptions: [],
      backgroundColor: "#fff",
      hoverBackgroundColor: "#f3f3f3",
      border: "none",
      borderRadius: 0,
      textColor: "#000",
    };
  },
  components: {},
  props: ["config"],
  computed: {},
  methods: {
    toggleRiskLevels() {
      this.isBottomSectionToggled = !this.isBottomSectionToggled;
    },
    setCurrentSelectedOption(option) {
      this.$emit("setSelectedOption", option);
    },
    setConfigData() {
      this.configOptions = this.config.options;
      this.width = this.config.width ?? this.width;
      this.placeholder = this.config.placeholder;
      if (this.config.backgroundColor) {
        this.backgroundColor = this.config.backgroundColor;
      }
      if (this.config.border) {
        this.border = this.config.border;
      }
      if (this.config.hoverBackgroundColor) {
        this.hoverBackgroundColor = this.config.hoverBackgroundColor;
      }
      if (this.config.textColor) {
        this.textColor = this.config.textColor;
      }
      if (this.config.borderRadius) {
        this.borderRadius = this.config.borderRadius;
      }
    },
    setOptionsHeight() {
      this.optionsHeight = this.optionHeight * this.configOptions.length;
    },
  },
  created() {
    this.setConfigData();
    this.setOptionsHeight();
  },
  beforeUdate() {
    // this.setOptionsHeight();
  },
  mounted() {},
};
</script>

<style lang="scss" scoped>
// * * binded style variables * *
// --options-height --> dynamic options menu height
// --option-height --> dynamic single option item height
// --dropdown-width --> dynamic dropdown & options drawer width

// * * * Variables * * *
$default-border-radius: 4px;
$default-text-hover-color: black;
$default-hover-color: #f3f3f3;
$default-text-color: #fff;

.ezi-header .dropdown {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: var(--dropdown-width);
  height: var(--option-height);
  padding: 0 5px;
  position: relative;
  border: var(--dropdown-border);
  border-radius: var(--main-el-border-radius);
  cursor: pointer;
  color: var(--dropdown-default-text-color);
  background: var(--dropdown-background-color);
  user-select: none;
  text-transform: uppercase;

  &.dropdown-active .angle-down {
    transform: rotate(-135deg);
    transform-origin: 6px 6px;
  }

  .angle-down {
    transition: transform 0.3s ease;
    display: flex;
    justify-content: center;
    align-items: center;
    border: solid $default-text-color;
    background: transparent;
    border-width: 0 2px 2px 0;
    padding: 4px;
    width: 1px;
    height: 1px;
    margin: -3px 5px 5px 5px;
    border-color: var(--ezi-primary-color);
    transform-origin: 5px 5px;
    transform: rotate(45deg);
  }

  .text {
    display: flex;
  }

  .text::v-deep span,
  .options .option p {
    font-family: "Raleway", sans-serif !important;
    font-size: 15px;
    color: rgb(128, 128, 128);
    text-transform: uppercase;
  }

  .options {
    display: flex;
    flex-direction: column;
    width: 100%;
    margin-bottom: calc(-1 * var(--options-height) - 3px);
    position: absolute;
    left: 0;
    top: calc(var(--option-height));
    z-index: 1;
    background: var(--dropdown-background-color);
    border: var(--dropdown-border);
    border-radius: var(--main-el-border-radius);
    padding: 5px 0;

    .option {
      display: flex;
      align-items: center;
      justify-content: flex-start;
      width: auto;
      padding: 0 5px;
      height: 35px;
    }

    .option:hover {
      color: $default-text-hover-color;
      background: $default-hover-color;
      transition: all 0.7s;
    }

    .option:last-child {
      border-radius: 0 0 4px 4px;
    }
  }

  .shown {
    border: var(--dropdown-border);
    max-height: 999px;
    transition: all 1s linear;
  }

  @media screen and (max-width: 1280px) {
    &.sf-header-navigation-item__item {
      display: flex;
      justify-content: center;
      padding: 16px;
      color: rgba(0, 0, 0, 0.4);
      height: var(--button-height);
      width: 100%;
      border-bottom: solid 1px #f1f2f3;
      font-weight: 500;
      line-height: 22.4px;

      .angle-down {
        margin: -3px 0px 0px 5px;
      }

      .text::v-deep img {
        height: 12px;
        margin-top: 5px;
      }

      .options {
        top: 54.4px;

        .option {
          justify-content: center;
        }
      }
    }
  }
}

.dropdown:hover {
  background: var(--dropdown-hover-background-color);
  transition: 0.7s all;
  cursor: pointer;

  .text,
  i {
    color: var(--dropdown-default-text-color);
  }
}

.dropdown-shown {
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  border-bottom-left-radius: 0;
  border-bottom-right-radius: 0;
}
</style>
