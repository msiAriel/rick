<script setup>
import ListApi from './pages/ListApi.vue'
import { onMounted, ref } from 'vue'
import { initFlowbite } from 'flowbite'
import AppLayout from './Layouts/AppLayout.vue'
import DataService from './services/DataService'

const page = ref(1);
const info = ref([]);
// crea la instancia del objeto
const service = new DataService()
const characters = ref([])
// obtiene los datos
const data = service.getData()

const fetchData = async () => {
  // prepara los datos 
  await service.fetchData(page.value)
  info.value = data.value.info
  characters.value = data.value.results
}


const changePage = (newPage) => {
  page.value = newPage
  fetchData()
}

onMounted( () => {

  fetchData();
  
  initFlowbite();
})
</script>

<template>
  <AppLayout>
    {{ info }}
    <section class="bg-white dark:bg-gray-900">
      <ListApi :characters="characters"></ListApi>
      <div class="flex flex-col items-center bg-white">
        <!-- Help text -->
        <span class="text-sm text-gray-700 dark:text-gray-400">
          Pagina <span class="font-semibold text-gray-900 dark:text-white">{{ page }}</span> de <span
            class="font-semibold text-gray-900 dark:text-white">{{ info.pages }}</span> con <span
            class="font-semibold text-gray-900 dark:text-white">{{ info.count }}</span> registros
        </span>
        <!-- Buttons -->
        <div class="inline-flex mt-2 xs:mt-0">
          <button @click="changePage(page - 1)"
            class="flex items-center justify-center px-4 h-10 text-base font-medium text-white bg-gray-800 rounded-s hover:bg-gray-900 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">
            Prev
          </button>
          <button @click="changePage(page + 1)"
            class="flex items-center justify-center px-4 h-10 text-base font-medium text-white bg-gray-800 border-0 border-s border-gray-700 rounded-e hover:bg-gray-900 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white">
            Next
          </button>
        </div>
      </div>
    </section>

  </AppLayout>
</template>
