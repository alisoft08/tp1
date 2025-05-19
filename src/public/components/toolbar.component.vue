<script>
import LanguageSwitcher from "./language-switcher.component.vue";
import {$t} from "@primeuix/styled";

export default {
  name: "toolbar-content",
  components: {LanguageSwitcher},
  methods: {
    $t,
    goHome() {
      this.$router.push({name: "home"});
    },
  },
  data() {
    return {
      items: [
        {label: 'home', to: '/home'},
        {label: 'examiner-performance-overview', to: '/nursing/examiner-performance-overview'},
      ],
    }
  }
}
</script>

<template>
  <pv-toolbar class="bg-primary" style="border-radius: 0;">
    <template #start>
      <div class="flex align-items-center gap-2 w-fit">
        <img src="https://hign.org/sites/default/files/2022-01/HIGN-logo-1200.png" alt="HIGN Logo" width="100px"
             class="mb-1">
        <p class="ml-3 font-bold">
          {{ this.$t('app-name') }}
        </p>
      </div>

    </template>

    <template #end>
      <div class="flex align-items-center gap-2 w-fit">
        <div class="flex-column">
          <router-link v-for="item in items" :key="item.label" v-slot="{navigate, href}" :to="item.to" custom>
            <pv-button :href="href" class="p-button-text text-white" @click="navigate">
              {{ $t(item.label) }}
            </pv-button>
          </router-link>
        </div>
        <language-switcher/>
      </div>

    </template>
  </pv-toolbar>
</template>

<style scoped>
</style>