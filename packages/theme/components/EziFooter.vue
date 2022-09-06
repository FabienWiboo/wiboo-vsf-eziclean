<template>
  <footer
    class="ezi-footer full-width"
    data-vcomponent="Footer"
  >
    <div class="newsletter">
      <h3 class="newsletter__title">RECEVEZ NOTRE NEWSLETTER</h3>
      <form class="newsletter__form" action="">
        <input type="email" class="email" required name="email" />
        <div class="rgpd">
          <div class="rgpd__container">
            <input type="checkbox" name="consent-rgpd" />
            <label for="consent-rgpd">
              J'accepte la
              <a href="#" class="rgpd-learn-more"
                >politique de confidentialité</a
              >
            </label>
          </div>
          <button type="submit" class="submit-newsletter">OK</button>
        </div>
      </form>
    </div>
    <div class="h-separator"></div>
    <!-- <div class="social-networks">
      <a class="social-networks__link" href="#">
        <img
          class="social-networks__icon"
          src="/theme-icons/Facebook.png"
          alt="Facebook"
        />
      </a>
      <a class="social-networks__link" href="#">
        <img
          class="social-networks__icon"
          src="/theme-icons/Instagram.png"
          alt="Instagram"
        />
      </a>
      <a class="social-networks__link" href="#">
        <img
          class="social-networks__icon"
          src="/theme-icons/Linkedin.png"
          alt="LinkedIn"
        />
      </a>
      <a class="social-networks__link" href="#">
        <img
          class="social-networks__icon"
          src="/theme-icons/Youtube.png"
          alt="Youtube"
        />
      </a>
    </div> -->
    <div class="website-information">
      <ul v-if="footerMenu">
        <li v-for="item in footerMenu">
          <a :href="'/page/'+item.permalien">{{ item.title }}</a>
        </li>
      </ul>
    </div>
    <!--<div class="project-version">EZIShop v1.5</div>-->

  </footer>
</template>

<script>
import {
  useContext,
} from "@nuxtjs/composition-api";
import { onSSR } from "@vue-storefront/core";
import { useContent } from '@wiboo/ezishop';

export default {
  name: "EziFooter",
  setup() {

    const { app: { i18n } } = useContext();
    const locale =
      i18n.locales.find(({ iso, code }) => [iso, code].includes(i18n.locale)) ||
      {};

    const {
      getFooterMenu,
      resFooterMenu
    } = useContent();

    onSSR(async () => {
      await getFooterMenu();
    });

    return {
      locale,
      footerMenu: resFooterMenu
    };
  },
};
</script>

<style lang="scss" scoped>
footer {
  background: #f4f4f4;
  padding: 50px 0;

  .project-version {
    text-align: center;
    margin-top: 30px;

  }

  & a,
  & label {
    color: rgba(0, 0, 0, 0.6);
  }

  & > div {
    width: 85%;
    margin: auto;
  }

  .newsletter {
    &__title {
      text-align: center;
      font-weight: 700;
      font-size: 24px;
    }

    &__form {
      .email {
        display: block;
        width: 25%;
        min-width: 300px;
        background-color: #f9f9f9;
        font-size: 1rem;
        line-height: 1.4;
        margin: 10px auto;
        padding: 8px;
        border: 2px solid rgba(0, 0, 0, 0.12);
        border-radius: 3px;
        outline: none;
      }

      .rgpd {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-bottom: 30px;

        &__container {
          .rgpd-learn-more {
            color: #0085ad;
            text-decoration: underline;
            transition: all 0.2s ease;

            &:hover {
              color: #48bfe4;
            }
          }
        }
      }

      .submit-newsletter {
        background-color: black;
        border: 1px black solid;
        transition: all 0.2s ease;
        color: white;
        font-size: 18px;
        font-weight: bold;
        letter-spacing: 1px;
        padding: 10px 15px;
        margin-left: 10px;

        &:hover {
          background-color: white;
          color: black;
        }
      }
    }
  }

  .h-separator {
    width: 80vw;
    border-color: rgba(0, 0, 0, 0.12);
    border-width: 2px;
  }

  .social-networks {
    padding-top: 10px;

    &__link {
      margin-right: 5px;
    }

    &__icon {
      width: 40px;
    }
  }

  .website-information {
    margin-top: 30px;
    width: 60vw;

    ul {
      display: flex;
      width: 100%;
      justify-content: space-between;
      text-align: center;
      gap: 20px;

      li {
        list-style: none;
        font-size: 18px;
      }
    }
  }
}

@media screen and (max-width: 1240px) {
  footer {
    .social-networks {
      margin: auto;
      width: max-content;
      margin-bottom: 20px;
    }

    .website-information {
      ul {
        flex-direction: column;
      }
    }
  }
}

@media screen and (max-width: 768px) {
  .rgpd {
    flex-direction: column;
    gap: 10px;
  }
}
</style>
