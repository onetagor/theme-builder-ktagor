<template>
  <nav class="top-bar border-b border-gray-200 sticky top-0 z-50 backdrop-blur-sm bg-white/80">
    <div class="container mx-auto px-6">
      <div class="flex items-center justify-between h-20">
        
        <!-- Left: Logo -->
        <div class="flex items-center space-x-4">
          <div>
            <h1 class="text-2xl font-black">YourBrand</h1>
            <p class="text-sm text-gray-500">Professional Solutions</p>
          </div>
        </div>

        <!-- Right: Desktop Nav -->
        <div class="hidden md:flex items-center space-x-8">
          <a href="#" class="text-gray-700 hover:text-purple-600 font-semibold transition-colors duration-300 relative group">
            🏠 Home
            <span class="absolute bottom-0 left-0 w-0 h-0.5 bg-purple-600 transition-all duration-300 group-hover:w-full"></span>
          </a>
          <a href="#" class="text-gray-700 hover:text-purple-600 font-semibold transition-colors duration-300 relative group">
            💼 Services
            <span class="absolute bottom-0 left-0 w-0 h-0.5 bg-purple-600 transition-all duration-300 group-hover:w-full"></span>
          </a>
          <a href="#" class="text-gray-700 hover:text-purple-600 font-semibold transition-colors duration-300 relative group">
            📊 Portfolio
            <span class="absolute bottom-0 left-0 w-0 h-0.5 bg-purple-600 transition-all duration-300 group-hover:w-full"></span>
          </a>
          <a href="#" class="text-gray-700 hover:text-purple-600 font-semibold transition-colors duration-300 relative group">
            📞 Contact
            <span class="absolute bottom-0 left-0 w-0 h-0.5 bg-purple-600 transition-all duration-300 group-hover:w-full"></span>
          </a>
        </div>

        <!-- Right: Mobile Menu Icon -->
        <div class="md:hidden">
          <button class="text-gray-700 hover:text-purple-600 focus:outline-none focus:ring-2 focus:ring-purple-500 rounded-lg p-2">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path>
            </svg>
          </button>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
const props = defineProps({
  sections: {
    type: Array,
    default: () => []
  }
})
</script>

<style scoped>
.top-bar {
  position: sticky;
  top: 0;
  z-index: 50;
}
</style>
