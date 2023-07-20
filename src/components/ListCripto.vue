<script setup>
import { ref, onMounted, computed } from 'vue';

const listCripto = ref([]);
const currentPage = ref(1); //variable reactiva para numero de pagina actual
const itemsPerPage = ref(5);//varibale para numero de elementos por pagina
const searchText = ref(''); //almacena la busqueda actual


//propiedad computada para el total de paginas
const totalPages = computed(() => Math.ceil(listCripto.value.length / itemsPerPage.value));

//elementos a mostrar en la pagina actual
const displayedItems = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage.value;
    /* Se le resta 1 a currentPage.value al calcular el valor de start porque los índices de las matrices en JavaScript comienzan en 0, mientras que el número de página comienza en 1. Al restar 1, se convierte el número de página en un índice de matriz válido.
    Por ejemplo, si currentPage.value es 1 y itemsPerPage.value es 5, el valor de start será (1 - 1) * 5 = 0, lo que significa que la primera página comenzará en el índice 0 de listCripto.value. Si currentPage.value es 2, el valor de start será (2 - 1) * 5 = 5, lo que significa que la segunda página comenzará en el índice 5 de listCripto.value, y así sucesivamente.
    */
    const end = start + itemsPerPage.value;
    /* 
    El método slice se utiliza para obtener una subsección de una matriz. Toma dos argumentos: el índice inicial y el índice final de la subsección que se desea obtener. Devuelve una nueva matriz que contiene solo los elementos entre los índices especificados.
    En el código que proporcionaste, se utiliza slice para obtener una subsección de listCripto.value que contenga solo los elementos entre los índices start y end. Esto da como resultado una nueva matriz que contiene solo los elementos que se deben mostrar en la página actual de la tabla.
    */
    return listCripto.value.slice(start, end);
});

//propiedad computada que muestra la cripto
const filteredItems = computed(() => {
  if (!searchText.value) {
    return listCripto.value;
  }
  return listCripto.value.filter(cripto =>
    cripto.CoinInfo.FullName.toLowerCase().includes(searchText.value.toLowerCase())
  );
});

//una propiedad computada para devolver displayedItems o filteredItems dependiendo del valor de searchText
const itemsToDisplay = computed(() => {
  if (!searchText.value) {
    return displayedItems.value;
  }
  return filteredItems.value;
});



onMounted(() => {
    const url = 'https://min-api.cryptocompare.com/data/top/mktcapfull?limit=25&tsym=USD';
    fetch(url)
    .then(response => response.json())
    .then(data => {
        listCripto.value = data.Data;
      /*   console.log(listCripto.value)
        listCripto.value.forEach(cripto => {
            console.log(cripto.CoinInfo.FullName)
        }) */
    })
});

function nextPage() {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
}

function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
}


</script>

<template>
    <div class="min-h-[300px] bg-gray-900 rounded-lg px-7 py-7">
        <div>
            <h2 class="font-medium text-xl">Criptomonedas - seguimiento</h2>
            <p class="text-gray-500 mt-1">Actualizado: 19 de junio del 2023</p>
        </div>
        <div class="rounded-lg border border-gray-500 hover:border-purple-500 transition-colors px-2 pb-1 pt-[2px] w-96 mt-5">
            <p class="text-sm text-gray-500">Buscar ahora</p>
            <form>
                 <input 
                 type="text" 
                 class="p-1 w-full border-none bg-transparent focus:outline-none"
                 @input="searchText = $event.target.value">
            </form>
        </div>

        <table class="table-auto w-full mt-7">
            <thead class="text-left border-t border-b border-gray-500">
                <tr>
                    <th class="px-4 py-5 font-normal text-sm text-gray-400">
                        <div class="inline-flex items-center gap-2">
                            <span>Nombre</span>
                            <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-red-500 w-5" viewBox="0 0 24 24"><path d="M19 2H5c-1.103 0-2 .897-2 2v12c0 1.103.897 2 2 2h3.5l3.5 4 3.5-4H19c1.103 0 2-.897 2-2V4c0-1.103-.897-2-2-2zM9 12a2 2 0 1 1 .001-4.001A2 2 0 0 1 9 12zm6 0a2 2 0 1 1 .001-4.001A2 2 0 0 1 15 12z"></path></svg>
                        </div>
                    </th>
                    <th class="px-4 py-5 font-normal text-sm text-gray-400">
                        <div class="inline-flex items-center gap-2">
                            <span>Precio</span>
                            <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-green-500 w-5" viewBox="0 0 24 24"><path d="M12 2C6.486 2 2 6.486 2 12s4.486 10 10 10 10-4.486 10-10S17.514 2 12 2zm1 14.915V18h-2v-1.08c-2.339-.367-3-2.002-3-2.92h2c.011.143.159 1 2 1 1.38 0 2-.585 2-1 0-.324 0-1-2-1-3.48 0-4-1.88-4-3 0-1.288 1.029-2.584 3-2.915V6.012h2v1.109c1.734.41 2.4 1.853 2.4 2.879h-1l-1 .018C13.386 9.638 13.185 9 12 9c-1.299 0-2 .516-2 1 0 .374 0 1 2 1 3.48 0 4 1.88 4 3 0 1.288-1.029 2.584-3 2.915z"></path></svg>
                        </div>
                    </th>
                    <th class="px-4 py-5 font-normal text-sm text-gray-400">
                        <div class="inline-flex items-center gap-2">
                            <span>Más alto</span>
                            <svg xmlns="http://www.w3.org/2000/svg"  class="fill-current text-pink-500 w-5" viewBox="0 0 24 24"><path d="m2.6 13.083 3.452 1.511L16 9.167l-6 7 8.6 3.916a1 1 0 0 0 1.399-.85l1-15a1.002 1.002 0 0 0-1.424-.972l-17 8a1.002 1.002 0 0 0 .025 1.822zM8 22.167l4.776-2.316L8 17.623z"></path></svg>
                        </div>
                    </th>
                    <th class="px-4 py-5 font-normal text-sm text-gray-400">
                        <div class="inline-flex items-center gap-2">
                            <span>Variación</span>
                            <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-blue-600 w-5" viewBox="0 0 24 24"><path d="M20 12a2 2 0 0 0-.703.133l-2.398-1.963c.059-.214.101-.436.101-.67C17 8.114 15.886 7 14.5 7S12 8.114 12 9.5c0 .396.1.765.262 1.097l-2.909 3.438A2.06 2.06 0 0 0 9 14c-.179 0-.348.03-.512.074l-2.563-2.563C5.97 11.348 6 11.179 6 11c0-1.108-.892-2-2-2s-2 .892-2 2 .892 2 2 2c.179 0 .348-.03.512-.074l2.563 2.563A1.906 1.906 0 0 0 7 16c0 1.108.892 2 2 2s2-.892 2-2c0-.237-.048-.46-.123-.671l2.913-3.442c.227.066.462.113.71.113a2.48 2.48 0 0 0 1.133-.281l2.399 1.963A2.077 2.077 0 0 0 18 14c0 1.108.892 2 2 2s2-.892 2-2-.892-2-2-2z"></path></svg>
                        </div>
                    </th>
                    <th class="px-4 py-5 font-normal text-sm text-gray-400">
                        <div class="inline-flex items-center gap-2">
                            <span>Logotipo</span>
                            <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-yellow-500 w-5" viewBox="0 0 24 24"><path d="M3 3v17a1 1 0 0 0 1 1h17v-2H5V3H3z"></path><path d="M15.293 14.707a.999.999 0 0 0 1.414 0l5-5-1.414-1.414L16 12.586l-2.293-2.293a.999.999 0 0 0-1.414 0l-5 5 1.414 1.414L13 12.414l2.293 2.293z"></path></svg>
                        </div>
                    </th>
                </tr>
            </thead>
            <tbody v-for="cripto in itemsToDisplay" :key="cripto.id"> 
                <tr class="hover:opacity-[0.6] transition-all">
                <td class="px-4 py-4">
                    <div class="inline-flex items-center gap-2">
                        <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-gray-400 w-7" viewBox="0 0 24 24"><path d="M12 2a10 10 0 1 0 10 10A10 10 0 0 0 12 2zm0 18a8 8 0 1 1 8-8 8 8 0 0 1-8 8z"></path><path d="M12 11c-2 0-2-.63-2-1s.7-1 2-1 1.39.64 1.4 1h2A3 3 0 0 0 13 7.12V6h-2v1.09C9 7.42 8 8.71 8 10c0 1.12.52 3 4 3 2 0 2 .68 2 1s-.62 1-2 1c-1.84 0-2-.86-2-1H8c0 .92.66 2.55 3 2.92V18h2v-1.08c2-.34 3-1.63 3-2.92 0-1.12-.52-3-4-3z"></path></svg>
                        <p class="text-md font-semibold max-w-[120px]">{{ cripto.CoinInfo.FullName }}</p>
                    </div>
                </td>
                <td class="px-4 py-4"><span class="text-[10px] p-1 font-medium rounded-sm bg-neutral-950 text-cyan-300">USD</span> {{ cripto.DISPLAY.USD.PRICE }}</td> 
                <td class="px-4 py-4"><span class="text-[10px] p-1 font-medium rounded-sm bg-neutral-950 text-yellow-400">USD</span> {{ cripto.DISPLAY.USD.HIGHDAY }}</td> 
                <td class="px-4 py-4 text-green-400">% {{ cripto.DISPLAY.USD.CHANGEPCT24HOUR }}</td> 
                <td class="px-4 py-4">
                    <img class="w-10" :src="'https://cryptocompare.com/' + cripto.CoinInfo.ImageUrl" alt="cripto imagge">
                </td>
                </tr>
            </tbody>
        </table>

        <!-- paginador -->
        <div class="bg-gray-800 p-5 rounded-md mt-5 flex items-center justify-between">
            <p class="text-sm text-gray-400">Pagina {{ currentPage }} de {{ totalPages }}</p>
            <div class="flex items-center gap-7">
                <button  @click="prevPage" class="w-9 h-9 rounded-md border border-gray-400 hover:bg-purple-700 flex items-center justify-center">
                    <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-gray-400 w-5" viewBox="0 0 24 24"><path d="m8.121 12 4.94-4.939-2.122-2.122L3.879 12l7.06 7.061 2.122-2.122z"></path><path d="M17.939 4.939 10.879 12l7.06 7.061 2.122-2.122L15.121 12l4.94-4.939z"></path></svg>                </button>
                <button @click="nextPage" class="w-9 h-9 rounded-md border border-gray-400 hover:bg-purple-700 flex items-center justify-center">
                    <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-gray-400 w-5" viewBox="0 0 24 24"><path d="m13.061 4.939-2.122 2.122L15.879 12l-4.94 4.939 2.122 2.122L20.121 12z"></path><path d="M6.061 19.061 13.121 12l-7.06-7.061-2.122 2.122L8.879 12l-4.94 4.939z"></path></svg>
                </button>
            </div>
        </div>

    </div>
</template>
