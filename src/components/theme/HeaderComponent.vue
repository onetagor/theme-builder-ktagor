<template>
  <div class="header relative bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900 py-24 overflow-hidden">
    <!-- Background Effects -->
    <div class="absolute inset-0 bg-gray-900 opacity-50"></div>
    <div class="absolute inset-0">
      <div class="absolute inset-0 bg-gradient-to-r from-purple-600/20 to-pink-600/20"></div>
      <div class="absolute top-0 left-0 w-full h-full bg-gradient-radial from-purple-500/30 to-transparent"></div>
    </div>
    
    <div class="container mx-auto px-6 text-center relative z-10">
      <div v-if="bannerImage" class="mb-12">
        <div class="relative inline-block">
          <img 
            :src="bannerImage" 
            :alt="title"
            class="mx-auto max-h-40 w-auto rounded-2xl shadow-2xl border-2 border-white/20"
          />
          <div class="absolute inset-0 rounded-2xl bg-gradient-to-t from-black/20 to-transparent"></div>
        </div>
      </div>
      
      <h1 class="text-5xl md:text-7xl font-black text-white mb-6 leading-tight">
        {{ title }}
        <span class="block text-2xl md:text-3xl font-normal text-purple-300 mt-2">✨ Professional Excellence</span>
      </h1>
      
      <p v-if="tagline" class="text-xl md:text-2xl text-slate-300 max-w-3xl mx-auto mb-12 leading-relaxed">
        {{ tagline }}
      </p>

      <!-- Call to Action Buttons -->
      <div class="flex flex-col sm:flex-row gap-6 justify-center items-center mb-16">
        <button class="bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 text-white px-12 py-4 rounded-full font-bold text-lg transition-all duration-300 shadow-xl hover:shadow-2xl hover:scale-105 transform">
          🚀 Get Started Now
        </button>
        <button class="border-2 border-white/30 text-white hover:bg-white/10 px-12 py-4 rounded-full font-bold text-lg transition-all duration-300 backdrop-blur-sm hover:border-white/50">
          📖 Learn More
        </button>
      </div>

      <!-- Feature Highlights -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-6xl mx-auto">
        <div class="text-center bg-white/10 backdrop-blur-sm rounded-2xl p-8 border border-white/20 hover:bg-white/20 transition-all duration-300">
          <div class="bg-gradient-to-r from-purple-500 to-pink-500 w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-4 shadow-lg">
            <span class="text-2xl">⚡</span>
          </div>
          <h3 class="text-xl font-bold text-white mb-3">Lightning Fast</h3>
          <p class="text-slate-300">Built for speed with optimized performance and instant loading</p>
        </div>

        <div class="text-center bg-white/10 backdrop-blur-sm rounded-2xl p-8 border border-white/20 hover:bg-white/20 transition-all duration-300">
          <div class="bg-gradient-to-r from-green-500 to-blue-500 w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-4 shadow-lg">
            <span class="text-2xl">🛡️</span>
          </div>
          <h3 class="text-xl font-bold text-white mb-3">Ultra Secure</h3>
          <p class="text-slate-300">Enterprise-grade security with 99.9% uptime guarantee</p>
        </div>

        <div class="text-center bg-white/10 backdrop-blur-sm rounded-2xl p-8 border border-white/20 hover:bg-white/20 transition-all duration-300">
          <div class="bg-gradient-to-r from-orange-500 to-red-500 w-16 h-16 rounded-full flex items-center justify-center mx-auto mb-4 shadow-lg">
            <span class="text-2xl">🎯</span>
          </div>
          <h3 class="text-xl font-bold text-white mb-3">Easy to Use</h3>
          <p class="text-slate-300">Intuitive interface designed for users of all experience levels</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
    default: 'Welcome to Our Amazing Website'
  },
  tagline: {
    type: String,
    default: 'We help businesses grow with innovative solutions and exceptional service. Join thousands of satisfied customers.'
  },
  bannerImage: {
    type: String,
    default: ''
  }
})
</script>

<style scoped>
.bg-gradient-radial {
  background: radial-gradient(circle at center, rgba(120, 119, 198, 0.3) 0%, transparent 70%);
}
</style>
