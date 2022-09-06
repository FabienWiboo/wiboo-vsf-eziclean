<template>
    <div>
        <h1>intégration d'un loader ici à prévoir</h1>
        <span v-if="redirectWooCartLink" ></span>
    </div>
</template>
<style lang="scss" scoped></style>
<script>
import { onSSR } from "@vue-storefront/core";
import { useWooCart } from '@wiboo/ezishop';
import { useContext, computed } from "@nuxtjs/composition-api";
export default {
    name: "Checkout",
    setup() {
        
        const { redirect } = useContext() ;
        const { result, convertCartInWoo } = useWooCart();

        onSSR(async () => {
            await convertCartInWoo();
        });

        const redirectWooCartLink = computed(() => {
            if (result.value) {
                return process.env.SHOP_URL+'vsf_cookie_cart.php?' + Object.keys(result.value).map(key => key + '=' + result.value[key]).join('&')
            }
        });

        return {
            order: result,
            redirectWooCartLink
        };

    },
    updated(){
        console.log('UPDATED',this.order);
        if(this.redirectWooCartLink){
            window.location.href = this.redirectWooCartLink;
        }
    }
};
</script>