<template>
    <div>
      <button @click="openModal" class="bg-indigo-600 text-white px-4 py-2 rounded">Add Tile</button>
  
      <transition name="fade">
        <div v-if="isModalOpen" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50" @click.self="closeModal">
          <div class="bg-white p-6 rounded shadow-md w-full max-w-md">
            <div class="flex justify-between items-center mb-4">
              <h2 class="text-xl font-semibold">Add New Tile</h2>
              <button @click="closeModal" class="text-gray-700 hover:text-gray-900">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
                </svg>
              </button>
            </div>
            <form @submit.prevent="submitTile">
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-900">Upload Image</label>
                <div class="relative">
                  <input 
                    v-if="!newTile.Image"
                    type="file" 
                    @change="onImageSelected" 
                    class="absolute inset-0 w-full h-full opacity-0 cursor-pointer" 
                    required 
                  />
                  <div 
                    class="border-2 border-dashed border-gray-300 rounded h-48 flex items-center justify-center"
                    :class="{'bg-gray-100': !newTile.Image, 'bg-cover': newTile.Image}"
                    :style="newTile.Image ? 'background-image: url(' + newTile.Image + ')' : ''"
                  >
                    <span v-if="!newTile.Image" class="text-gray-400 text-2xl">+</span>
                  </div>
                </div>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-900">Name</label>
                <input v-model="newTile.Name" type="text" placeholder="Name" class="block w-full p-2 border rounded" required>
              </div>
              <div class="mb-4">
                <label class="block mb-2 text-sm font-medium text-gray-900">Description</label>
                <input v-model="newTile.Description" type="text" placeholder="Description" class="block w-full p-2 border rounded" required>
              </div>
              <div class="mb-4 relative">
                <label class="block mb-2 text-sm font-medium text-gray-900">Price</label>
                <input v-model="newTile.Price" @input="validatePrice" type="text" placeholder="Price" :class="priceInputClass" class="block w-full p-2 border rounded pr-10">
                <div v-if="isPriceValid" class="absolute inset-y-0 right-0 pr-3 pt-5 flex items-center pointer-events-none">
                  <svg class="h-6 w-6 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                  </svg>
                </div>
                <div v-if="!isPriceValid && newTile.Price !== ''" class="absolute inset-y-0 right-0 pr-3 pt-5 flex items-center pointer-events-none">
                  <svg class="h-6 w-6 text-red-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                  </svg>
                </div>
              </div>
              <button type="submit" class="w-full bg-indigo-600 text-white px-4 py-2 rounded">Save</button>
            </form>
          </div>
        </div>
      </transition>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  const emits = defineEmits(['add-tile']);
  
  const newTile = ref({
    Name: '',
    Description: '',
    Image: '',
    Price: '',
  });
  
  const isModalOpen = ref(false);
  const isPriceValid = ref();
  
  const openModal = () => {
    isModalOpen.value = true;
  };
  
  const closeModal = () => {
    isModalOpen.value = false;
  };
  
  const submitTile = () => {
    if (isPriceValid.value) {
      emits('add-tile', { ...newTile.value });
      newTile.value = {
        Name: '',
        Description: '',
        Image: '',
        Price: '',
      };
      closeModal();
    }
  };
  
  const onImageSelected = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        newTile.value.Image = e.target.result;
      };
      reader.readAsDataURL(file);
    }
  };
  
  const validatePrice = () => {
    const price = newTile.value.Price;
    isPriceValid.value = price === '' || /^\d+(\.\d{1,2})?$/.test(price);
  };
  
  const priceInputClass = computed(() => {
    if (newTile.value.Price === '') {
      return 'border-gray-300';
    }
    return isPriceValid.value ? 'border-green-500' : 'border-red-500';
  });
  </script>
  
  <style scoped>
  .fade-enter-active, .fade-leave-active {
    transition: opacity 0.5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  </style>
  