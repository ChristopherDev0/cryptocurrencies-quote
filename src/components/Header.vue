
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
        <div class="py-4 px-14 border-b border-b-gray-700 bg-gray-950 bg-opacity-80 backdrop-filter backdrop-blur-sm flex items-center justify-between"> 
            <div class="flex items-center gap-5">
                <div class="rounded-full w-12 h-12 border-2 border-gray-600 flex items-center justify-center">
                    <svg xmlns="http://www.w3.org/2000/svg" class="fill-current text-purple-500 w-6" viewBox="0 0 24 24" ><path d="M3 11h8V3H3zm2-6h4v4H5zM3 21h8v-8H3zm2-6h4v4H5zm8-12v8h8V3zm6 6h-4V5h4zm-5.99 4h2v2h-2zm2 2h2v2h-2zm-2 2h2v2h-2zm4 0h2v2h-2zm2 2h2v2h-2zm-4 0h2v2h-2zm2-6h2v2h-2zm2 2h2v2h-2z"></path></svg>
                </div>
                <h1 class="font-bold text-2xl tracking-[1px]">Cripto<span class="font-light">currencies</span></h1>
            </div>
            <div>
                <div>
                    <button
                    type="button"
                    @click="openModal"
                    class="rounded-md bg-gray-800 px-4 py-2 text-sm font-medium text-white border border-gray-700"
                    >
                      Descripcion general
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
                            class="w-full max-w-md transform overflow-hidden rounded-2xl bg-white p-6 text-left align-middle shadow-xl transition-all"
                            >
                            <DialogTitle
                                as="h3"
                                class="text-lg font-medium leading-6 text-gray-900"
                            >
                                Payment successful
                            </DialogTitle>
                            <div class="mt-2">
                                <p class="text-sm text-gray-500">
                                Your payment has been successfully submitted. We’ve sent you
                                an email with all of the details of your order.
                                </p>
                            </div>

                            <div class="mt-4">
                                <button
                                type="button"
                                class="inline-flex justify-center rounded-md border border-transparent bg-blue-100 px-4 py-2 text-sm font-medium text-blue-900 hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2"
                                @click="closeModal"
                                >
                                Got it, thanks!
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

