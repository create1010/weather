<script setup>
import headerView from './layouts/headerView.vue';
import mainView from './views/mainView.vue';
import footerView from './layouts/footerView.vue';
</script>

<template>
  <div class="bg-[url('/img/bg.jpg')] bg-cover bg-no-repeat bg-center">
    <headerView />
    <mainView />
    <footerView />
  </div>
</template>
