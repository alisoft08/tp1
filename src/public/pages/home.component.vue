<script>
import {$t} from "@primeuix/styled";

export default {
  name: "home.component",
  methods: {$t}
}
</script>

<template>
  <div>
    <h1 class="text-center">
      {{ $t('home') }}
    </h1>
    <p class="text-center text-3xl ">
      {{ $t('home-info') }}
    </p>
  </div>


</template>

<style scoped>

</style>