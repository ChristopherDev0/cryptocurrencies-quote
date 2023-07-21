<script setup>
import Header from './components/Header.vue';
import ListCriptoVue from './components/ListCripto.vue';
import QuoterVue from './components/Quoter.vue';
import Footer from './components/Footer.vue'

</script>

<template>
  <Header></Header>
  <main class="mt-32 max-w-7xl lg:mx-auto mx-4 mb-20">
    <div class=" flex flex-col gap-10 lg:flex-row">
      <div class="lg:w-2/3">
        <ListCriptoVue></ListCriptoVue>
      </div>
      <div class="lg:w-1/3">
        <QuoterVue></QuoterVue>
      </div>
    </div>
  </main>
  <Footer></Footer>
</template>



