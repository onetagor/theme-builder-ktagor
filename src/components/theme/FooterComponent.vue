<template>
  <div class="footer bg-gradient-to-r from-gray-900 via-gray-800 to-gray-900 text-white py-16 relative overflow-hidden">
    <!-- Background Pattern -->
    <div class="absolute inset-0 bg-gray-900 opacity-50"></div>
    <div class="absolute inset-0">
      <div class="absolute inset-0 bg-gradient-to-b from-transparent to-black/30"></div>
    </div>
    
    <div class="container mx-auto px-6 relative z-10">
      <!-- Main Footer Content -->
      <div 
        class="grid gap-8 mb-12"
        :class="{
          'grid-cols-1 md:grid-cols-2': columns === 2,
          'grid-cols-1 md:grid-cols-3': columns === 3,
          'grid-cols-1 sm:grid-cols-2 lg:grid-cols-4': columns === 4
        }"
      >
        <div 
          v-for="(column, index) in content" 
          :key="index"
          class="footer-column"
        >
          <h3 class="text-2xl font-bold mb-6 text-white border-b-2 border-purple-500 pb-2 inline-block">
            {{ column.title }}
          </h3>
          <ul class="space-y-3">
            <li 
              v-for="(item, itemIndex) in column.items" 
              :key="itemIndex"
            >
              <a 
                href="#"
                class="text-gray-300 hover:text-purple-400 transition-colors duration-300 text-lg flex items-center group"
              >
                <span class="mr-2 text-purple-400 opacity-0 group-hover:opacity-100 transition-opacity">▶</span>
                {{ item }}
              </a>
            </li>
          </ul>
        </div>
      </div>
      
      <!-- Newsletter Signup -->
      <div class="bg-gradient-to-r from-purple-900/30 to-pink-900/30 backdrop-blur-sm rounded-2xl p-8 mb-12 border border-purple-500/20">
        <div class="text-center">
          <h3 class="text-2xl font-bold text-white mb-4">📧 Stay Updated</h3>
          <p class="text-gray-300 mb-6">Subscribe to our newsletter for the latest updates and exclusive offers</p>
          <div class="flex flex-col sm:flex-row gap-4 max-w-md mx-auto">
            <input 
              type="email" 
              placeholder="Enter your email"
              class="flex-1 px-6 py-3 rounded-full bg-white/10 backdrop-blur-sm border border-white/20 text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-transparent"
            />
            <button class="bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 text-white px-8 py-3 rounded-full font-bold transition-all duration-300 shadow-lg hover:shadow-xl transform hover:scale-105">
              Subscribe
            </button>
          </div>
        </div>
      </div>
      
      <!-- Bottom Footer -->
      <div class="border-t border-gray-700 pt-8">
        <div class="flex flex-col md:flex-row justify-between items-center gap-6">
          <div class="text-center md:text-left">
            <p class="text-gray-400 text-lg">
              © {{ currentYear }} <span class="text-purple-400 font-bold">Your Company</span>. All rights reserved.
            </p>
          </div>
          
          <!-- Social Links -->
          <div class="flex space-x-6">
            <a href="#" class="text-gray-400 hover:text-purple-400 transition-colors duration-300 text-2xl">
              📘
            </a>
            <a href="#" class="text-gray-400 hover:text-blue-400 transition-colors duration-300 text-2xl">
              🐦
            </a>
            <a href="#" class="text-gray-400 hover:text-pink-400 transition-colors duration-300 text-2xl">
              📸
            </a>
            <a href="#" class="text-gray-400 hover:text-blue-600 transition-colors duration-300 text-2xl">
              💼
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  columns: {
    type: Number,
    default: 4
  },
  content: {
    type: Array,
    default: () => [
      { 
        title: 'Company', 
        items: ['About Us', 'Our Team', 'Careers', 'Press', 'Partners'] 
      },
      { 
        title: 'Services', 
        items: ['Web Development', 'Mobile Apps', 'SEO Services', 'Digital Marketing', 'Consulting'] 
      },
      { 
        title: 'Resources', 
        items: ['Blog', 'Documentation', 'Help Center', 'Community', 'Tutorials'] 
      },
      { 
        title: 'Contact', 
        items: ['Support', 'Sales', 'Feedback', 'Partnerships', 'Media Inquiries'] 
      }
    ]
  }
})

const currentYear = computed(() => new Date().getFullYear())
</script>

<style scoped>
.footer {
  background: linear-gradient(135deg, #1f2937 0%, #111827 100%);
}

.footer-column h3 {
  color: #f9fafb;
  font-weight: 600;
}

.footer-column ul li a:hover {
  color: #60a5fa;
}
</style>
