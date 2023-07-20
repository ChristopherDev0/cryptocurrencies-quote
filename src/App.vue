<script setup>
import Header from './components/Header.vue';
import ListCriptoVue from './components/ListCripto.vue';
import QuoterVue from './components/Quoter.vue';

</script>

<template>
  <Header></Header>
  <main class="mt-32 max-w-7xl md:mx-auto mx-4 mb-20">
    <div class="flex items-start gap-10">
      <div class="w-2/3">
        <ListCriptoVue></ListCriptoVue>
      </div>
      <div class="w-1/3">
        <QuoterVue></QuoterVue>
      </div>
    </div>
  </main>
</template>

