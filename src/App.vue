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

<style>
/* Ancho del scrollbar */
::-webkit-scrollbar {
  width: 12px;
}

/* Track del scrollbar */
::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}

/* Handle del scrollbar */
::-webkit-scrollbar-thumb {
  background: #2d374b;
  border-radius: 10px;
}

/* Handle del scrollbar al hacer hover */
::-webkit-scrollbar-thumb:hover {
  background: #1a2233;
}

</style>

