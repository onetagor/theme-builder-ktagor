<template>
  <div class="logo-component py-8">
    <div class="container mx-auto px-4 text-center">
      <div v-if="type === 'text'" class="text-logo">
        <h2 class="text-3xl md:text-4xl font-bold text-gray-900 hover:text-blue-600 transition-colors cursor-pointer">
          {{ text }}
        </h2>
      </div>
      
      <div v-if="type === 'image' && image" class="image-logo">
        <img 
          :src="image" 
          :alt="text || 'Logo'"
          class="mx-auto max-h-16 w-auto hover:scale-105 transition-transform cursor-pointer"
        />
      </div>
      
      <div v-if="type === 'image' && !image" class="placeholder-logo">
        <div class="mx-auto w-32 h-16 bg-gray-200 rounded-lg flex items-center justify-center">
          <span class="text-gray-500 text-sm">Logo Image</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  type: {
    type: String,
    default: 'text' // 'text' or 'image'
  },
  text: {
    type: String,
    default: '🚀 TechCorp Solutions'
  },
  image: {
    type: String,
    default: ''
  }
})
</script>

<style scoped>
.logo-component {
  background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
}
</style>
