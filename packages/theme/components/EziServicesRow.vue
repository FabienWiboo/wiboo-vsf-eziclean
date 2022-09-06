<template>
  <div class="ezi-services-row full-width">
    <div class="service">
      <div>
        <img src="/theme-icons/services/garantie.png" alt="icon_garantie" />
      </div>
      <div>
        <h6 v-html="$t('service_row_title1')"></h6>
        <p v-html="$t('service_row_txt1')"></p>
      </div>
    </div>
    <div class="service">
      <div>
        <img src="/theme-icons/services/camion.png" alt="icon_camion" />
      </div>
      <div>
        <h6 v-html="$t('service_row_title2')"></h6>
        <p v-html="$t('service_row_txt2')"></p>
      </div>
    </div>
    <div class="service">
      <div>
        <img src="/theme-icons/services/monnaie.png" alt="icon_monnaie" />
      </div>
      <div>
        <h6 v-html="$t('service_row_title3')"></h6>
        <p v-html="$t('service_row_txt3')"></p>
      </div>
    </div>
  </div>
</template>
<script>
export default {};
</script>
<style lang="scss" scoped>
.ezi-services-row {
  background-color: #0a0a0a;
  display: flex;
  justify-content: space-between;
  padding: 25px 80px;

  & > .service {
    width: max-content;
    display: grid;
    grid-template-columns: max-content auto;

    & > div:nth-child(1) {
      display: flex;
      align-items: center;

      img {
        margin-right: 20px;
        margin-left: 30px;
        height: 70px;
      }
    }

    & > div:nth-child(2) {
      font-size: 16px;

      h6 {
        margin-bottom: 10px;
      }

      p,
      h6 {
        line-height: 1;
      }
    }
  }
}

a,
h6,
p {
  color: white;
  line-height: 1.6;
}

a {
  text-decoration: underline;
}

h6 {
  font-size: 20px;
  font-weight: 400;
}

@media screen and (max-width: 1024px) {
  .ezi-services-row {
    flex-direction: column;
    justify-content: center;
    gap: 30px;
    padding: 25px 0 10px 0;

    & > .service {
      display: flex;
      flex-direction: column;
      text-align: center;
      align-items: center;
      margin: auto;

      div:nth-child(1) {
        img {
          height: 50px;
          margin-bottom: 10px;
          margin-right: 0;
        }
      }
    }

    & > .service:last-child {
      margin-bottom: 15px;
    } 
  }
}
</style>