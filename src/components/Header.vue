
<script setup>
import { ref } from 'vue'
import {
  TransitionRoot,
  TransitionChild,
  Dialog,
  DialogPanel,
  DialogTitle,
} from '@headlessui/vue'

const isOpen = ref(true)

function closeModal() {
  isOpen.value = false
}
function openModal() {
  isOpen.value = true
}
</script>

<template>
    <div class="fixed top-0 left-0 right-0 z-10">            
        <div class="py-4 px-4 md:px-14 border-b border-b-gray-700 bg-gray-950 bg-opacity-70 backdrop-filter backdrop-blur-sm flex items-center justify-between"> 
            <div class="flex items-center gap-1">
                <img class="w-12 md:w-14" src="../assets/cripto.png" alt="image crypto">
                <!-- <div class="rounded-full w-12 h-12 border-2 border-gray-600 flex items-center justify-center">
                    <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-purple-500 w-6" viewBox="0 0 24 24" ><path d="M3 11h8V3H3zm2-6h4v4H5zM3 21h8v-8H3zm2-6h4v4H5zm8-12v8h8V3zm6 6h-4V5h4zm-5.99 4h2v2h-2zm2 2h2v2h-2zm-2 2h2v2h-2zm4 0h2v2h-2zm2 2h2v2h-2zm-4 0h2v2h-2zm2-6h2v2h-2zm2 2h2v2h-2z"></path></svg>
                </div> -->
                <h1 class="font-bold text-lg md:text-xl tracking-[1px]">Crypto<span class="font-light">currencies</span></h1>
            </div>
            <div>
                <div>
                    <button
                    type="button"
                    @click="openModal"
                    class="rounded-md bg-gray-800 px-4 py-2 text-xs md:text-sm font-medium text-white border border-gray-700"
                    >
                      Descripcion
                    </button>
                </div>
                <TransitionRoot appear :show="isOpen" as="template">
                    <Dialog as="div" @close="closeModal" class="relative z-10">
                    <TransitionChild
                        as="template"
                        enter="duration-300 ease-out"
                        enter-from="opacity-0"
                        enter-to="opacity-100"
                        leave="duration-200 ease-in"
                        leave-from="opacity-100"
                        leave-to="opacity-0"
                    >
                        <div class="fixed inset-0 bg-black bg-opacity-50" />
                    </TransitionChild>

                    <div class="fixed inset-0 overflow-y-auto">
                        <div
                        class="flex min-h-full items-center justify-center p-4 text-center"
                        >
                        <TransitionChild
                            as="template"
                            enter="duration-300 ease-out"
                            enter-from="opacity-0 scale-95"
                            enter-to="opacity-100 scale-100"
                            leave="duration-200 ease-in"
                            leave-from="opacity-100 scale-100"
                            leave-to="opacity-0 scale-95"
                        >
                            <DialogPanel
                            class="w-full max-w-xl transform overflow-hidden rounded-2xl bg-gray-200 p-6 text-left align-middle shadow-xl transition-all"
                            >
                            <DialogTitle as="h3">
                               <div class="flex items-center gap-2">
                                <img class="w-16" src="../assets/cripto.png" alt="image crypto">
                                <h3 class="text-gray-800 font-bold text-xl">Cryptocurrencies</h3>
                               </div>
                            </DialogTitle>
                            <div class="mt-2">
                                <p class="text-sm text-gray-500 bg-gray-100 p-5 shadow-lg rounded-lg border border-gray-100">
                                    Nuestra web es una plataforma informativa dedicada a las criptomonedas, donde los usuarios pueden explorar y descubrir el emocionante mundo de las monedas digitales. <span class="font-bold">Ofrecemos una amplia variedad de datos actualizados sobre diferentes criptomonedas</span>, incluyendo precios en tiempo real, busqueda en mercado, volumen de criptomonedas y cambios históricos.
                                </p>
                            </div>

                            <div class="mt-4">
                                <button
                                type="button"
                                class="inline-flex justify-center rounded-lg border border-transparent bg-gray-900 px-7 py-2 text-sm font-medium text-gray-200 hover:bg-gray-950 focus:outline-none"
                                @click="closeModal"
                                >
                                ¡Gracias!
                                </button>
                            </div>
                            </DialogPanel>
                        </TransitionChild>
                        </div>
                    </div>
                    </Dialog>
                </TransitionRoot>
            </div>
        </div>
    </div>
</template>

