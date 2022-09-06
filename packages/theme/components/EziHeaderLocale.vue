<template>
    <div class="sf-header-navigation-item nav-item">
        <EziSimpleDropdown
            :config="config"
            @setSelectedOption="setNewSelectedOption($event);"
        ></EziSimpleDropdown>
    </div>
</template>
<script>
import EziSimpleDropdown from "~/components/EziSimpleDropdown";
//import { getLanguages } from "~/utils/directus/DirectusService";

export default {
    components: {
        EziSimpleDropdown
    },
    data: function() {
        return {
            config: {
                options: this.$i18n.locales.filter(i => i.code !== this.$i18n.locale),
                placeholder: '<img src=\"/flags/'+this.$i18n.locale+'.png\" /><span>&nbsp;'+this.$i18n.locales.filter(i => i.code == this.$i18n.locale)[0].label+'</span>',
            }
        };
    },
    methods: {
        setNewSelectedOption(selectedOption) {
            this.config.placeholder = '<img src=\"/flags/'+selectedOption.code+'.png\" /><span>&nbsp;'+selectedOption.label+'</span>';
            //const newPath = this.switchLocalePath(selectedOption.code);
            this.$router.push(this.switchLocalePath(selectedOption.code));
        }
    }  
}
</script>