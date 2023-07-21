<script setup>
import { ref, reactive, onMounted } from 'vue';
import Spinner from './Spinner.vue';

const coins = ref([ //array para monedas
      { codigo: 'USD', texto: 'Dolar de Estados Unidos'},
      { codigo: 'MXN', texto: 'Peso Mexicano'},
      { codigo: 'EUR', texto: 'Euro'},
      { codigo: 'GBP', texto: 'Libra Esterlina'},
  ]);

  const error = ref('');
  const showSpinner = ref(false);

  const quoteData = ref({
    coin: '',
    cryptocurrencie: ''
});
 
  const cryptocurrencies = ref([]);
  const responseCryto = ref({});

  onMounted(() => {
    const url = 'https://min-api.cryptocompare.com/data/top/mktcapfull?limit=20&tsym=USD';
    fetch(url)
        .then(response => response.json())
        .then(data => {
            cryptocurrencies.value = data.Data;
        }) 
  })

const sendQuote = () => {
  if(Object.values(quoteData.value).includes('')){
     error.value = 'Todos los campos son obligarorios';
     setTimeout(() => {
        error.value = '';
     }, 3000);
    return
  }
  //cotizacion de criptos
  const {coin,cryptocurrencie } = quoteData.value;
  responseCryto.value = {};
    showSpinner.value = true;
    setTimeout(() => {
        const url = `https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${cryptocurrencie}&tsyms=${coin}`;
        fetch(url)
            .then(response => response.json())
            .then(data => {
                responseCryto.value = data.DISPLAY[cryptocurrencie][coin];
                showSpinner.value = false;
                 /*  console.log(responseCryto.value); */
        })
    }, 1000); 
}

</script>

<template>
   <div class="min-h-[300px] bg-gray-900 rounded-xl px-5 md:px-7 pb-8 pt-1">
        <div class="flex items-center justify-center">
            <img class="w-48" src="../assets/cripto.png" alt="image crypto">
        </div>
        <div>
            <h2 class="font-medium text-xl">Cotizar crypto</h2>
            <p class="text-gray-500 mt-1">En tiempo real</p>
        </div>
      <form @submit.prevent="sendQuote">
        <div class="rounded-lg border border-gray-500 hover:border-purple-500 transition-colors px-2 pb-1 pt-[2px] w-full mt-5">
            <p class="text-sm text-gray-500">Moneda</p>
            <select v-model="quoteData.coin" id="select" class="p-1 bg-gray-900 w-full border-none focus:outline-none">
                <option value=""></option>
                <option  v-for="coin in coins" :key="coin.id" :value="coin.codigo">{{ coin.texto }}</option>
            </select>
        </div>
        <div class="rounded-lg border border-gray-500 hover:border-purple-500 transition-colors px-2 pb-1 pt-[2px] w-full mt-5">
            <p class="text-sm text-gray-500">Criptomoneda</p>
            <select v-model="quoteData.cryptocurrencie" id="select" class="p-1 bg-gray-900 w-full border-none focus:outline-none">
                <option value=""></option>
                <option  v-for="crypto in cryptocurrencies" :key="crypto.id" :value="crypto.CoinInfo.Name">{{ crypto.CoinInfo.FullName }}</option>
            </select>
        </div>
        <div v-if="error !== ''">
            <p class="bg-gradient-to-tr from-red-600 mt-5 text-sm to-red-400 text-white text-center font-semibold rounded-md px-2 py-3">{{ error }}</p>
        </div>
        <input type="submit" value="Cotizar ahora" class="mt-7 inline-flex items-center hover:opacity-[0.7] justify-center font-semibold transition-all disabled:opacity-50 disabled:shadow-none disabled:pointer-events-none text-md w-full py-3 rounded-md cursor-pointer bg-gradient-to-tr from-purple-600 to-purple-400 text-white shadow-lg shadow-purple-500/40 active:opacity-[0.85]  px-4">
      </form>

      <div v-if="showSpinner" class="flex items-center justify-center mt-10">
        <Spinner></Spinner>
      </div>

      <div v-if="Object.keys(responseCryto).length > 0" class="bg-gray-800 mt-7 rounded-lg px-5 py-7 font-semibold">
         <div class="flex items-center justify-center"> 
            <img class="w-20" :src="'https://cryptocompare.com/' + responseCryto.IMAGEURL" alt="crypto photo">
         </div>
         <div class="flex items-center justify-between mt-4">
            <p class="">Precio actual:</p>
            <p class="text-purple-400">{{ responseCryto.PRICE }}</p>
         </div>
         <div class="flex items-center justify-between mt-1">
            <p class="">Precio mas alto:</p>
            <p class="text-purple-400">{{ responseCryto.HIGHDAY }}</p>
         </div>
         <div class="flex items-center justify-between mt-1">
            <p class="">Precio mas bajo:</p>
            <p class="text-purple-400">{{ responseCryto.LOWDAY }}</p>
         </div>
         <div class="flex items-center justify-between mt-1">
            <p class="">Variacion 24H:</p>
            <p class="text-purple-400">{{ responseCryto.CHANGEPCT24HOUR }} %</p>
         </div>
         <div class="flex items-center justify-between mt-1">
            <p class="">Actualizacion:</p>
            <p class="text-purple-400">{{ responseCryto.LASTUPDATE }}</p>
         </div>
      </div>
    </div>
</template>
