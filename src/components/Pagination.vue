<template>
  <div class="flex items-center justify-between bg-white px-4 py-3 sm:px-6">
    <div class="flex flex-1 justify-between sm:hidden">
      <button @click="changePage(currentPage - 1)" :disabled="currentPage === 1" class="relative inline-flex items-center border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">
        Previous
      </button>
      <button @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages" class="relative ml-3 inline-flex items-center border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">
        Next
      </button>
    </div>
    <div class="hidden sm:flex sm:flex-1 sm:items-center sm:justify-between">
      <div>
        <nav class="isolate inline-flex -space-x-px shadow-sm" aria-label="Pagination">
          <button @click="changePage(currentPage - 1)" :disabled="currentPage === 1" class="relative inline-flex items-center px-2 py-2 text-gray-400 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0">
            <span class="sr-only">Previous</span>
            <ChevronLeftIcon class="h-5 w-5" aria-hidden="true" />
          </button>
          <button
            v-for="page in pages"
            :key="page"
            @click="changePage(page)"
            :class="[
              'relative inline-flex items-center px-4 py-2 text-sm font-semibold',
              page === currentPage ? 'bg-indigo-600 text-white' : 'text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50'
            ]"
          >
            {{ page }}
          </button>
          <button @click="changePage(currentPage + 1)" :disabled="currentPage === totalPages" class="relative inline-flex items-center px-2 py-2 text-gray-400 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 focus:z-20 focus:outline-offset-0">
            <span class="sr-only">Next</span>
            <ChevronRightIcon class="h-5 w-5" aria-hidden="true" />
          </button>
        </nav>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ChevronLeftIcon, ChevronRightIcon } from '@heroicons/vue/20/solid';
import { computed, ref, watch } from 'vue';

const props = defineProps({
  currentPage: {
    type: Number,
    required: true
  },
  totalItems: {
    type: Number,
    required: true
  },
  itemsPerPage: {
    type: Number,
    required: true
  }
});

const emit = defineEmits(['update:currentPage']);

const totalPages = computed(() => Math.ceil(props.totalItems / props.itemsPerPage));
const pages = computed(() => Array.from({ length: totalPages.value }, (_, i) => i + 1));
const startItem = computed(() => (props.currentPage - 1) * props.itemsPerPage + 1);
const endItem = computed(() => Math.min(props.currentPage * props.itemsPerPage, props.totalItems));

const changePage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    emit('update:currentPage', page);
  }
};
</script>
