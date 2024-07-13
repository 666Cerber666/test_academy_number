<template>
  <div class="relative border p-4 rounded w-auto shadow-md">
    <div 
      @mouseenter="toggleDeleteIcon(true)" 
      @mouseleave="toggleDeleteIcon(false)" 
      @click="toggleDeleteIconOnMobile"
      class="relative"
    >
      <img :src="tile.Image" alt="Tile image" class="w-full h-auto rounded" />
      <transition name="fade">
        <div 
          v-if="showDeleteIcon" 
          class="absolute inset-0 bg-black bg-opacity-50 flex items-center justify-center rounded"
        >
            <TrashIcon @click.stop="deleteTile" class="h-8 w-8 text-white" />
        </div>
      </transition>
    </div>
    <div class="mt-2 text-center">
      <h3 class="text-lg font-semibold">{{ tile.Name }}</h3>
      <p class="text-sm text-gray-600">{{ tile.Description }}</p>
      <p class="text-lg font-bold">{{ tile.Price }} ₽</p>
    </div>
  </div>
</template>

<script>
import { TrashIcon } from '@heroicons/vue/20/solid'

export default {
  props: {
    tile: Object
  },
  components:{
    TrashIcon
  },
  data() {
    return {
      showDeleteIcon: false
    }
  },
  methods: {
    deleteTile() {
      this.$emit('delete-tile');
    },
    toggleDeleteIcon(status) {
      if (window.innerWidth >= 768) { // Desktop screen size
        this.showDeleteIcon = status;
      }
    },
    toggleDeleteIconOnMobile() {
      if (window.innerWidth < 768) { // Mobile screen size
        this.showDeleteIcon = !this.showDeleteIcon;
      }
    }
  }
}
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
