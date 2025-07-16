<template>
  <div class="left-sidebar bg-gradient-to-b from-white via-gray-50 to-white w-full border-r border-gray-200 shadow-lg">
    <div class="p-6">
      <!-- Navigation Header -->
      <div class="mb-8">
        <h2 class="text-2xl font-bold text-gray-900 mb-2">📋 Navigation</h2>
        <p class="text-gray-600 text-sm">Explore our main sections</p>
      </div>

      <!-- Dynamic Content -->
      <div class="space-y-6">
        <div 
          v-for="(item, index) in content" 
          :key="index"
          class="sidebar-item"
        >
          <div v-if="item.type === 'navigation'" class="navigation-section">
            <h3 class="text-xl font-bold text-gray-800 mb-4">{{ item.title || 'Navigation' }}</h3>
            <nav class="space-y-2">
              <a 
                v-for="(navItem, navIndex) in item.items" 
                :key="navIndex"
                :href="navItem.link"
                class="group flex items-center p-3 text-gray-700 hover:bg-gradient-to-r hover:from-purple-50 hover:to-pink-50 hover:text-purple-700 rounded-xl transition-all duration-300 border hover:border-purple-200"
              >
                <span class="mr-3 text-lg">🔗</span>
                <span class="font-medium">{{ navItem.text }}</span>
                <svg class="w-4 h-4 ml-auto text-gray-400 group-hover:text-purple-600 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
              </a>
            </nav>
          </div>
          
          <div v-if="item.type === 'text'" class="text-section">
            <div class="bg-gradient-to-r from-blue-50 to-indigo-50 p-4 rounded-xl border border-blue-200">
              <div class="prose text-gray-700 text-sm">
                {{ item.text }}
              </div>
            </div>
          </div>
          
          <div v-if="item.type === 'widget'" class="widget-section">
            <h3 class="text-xl font-bold text-gray-800 mb-4">{{ item.title }}</h3>
            <div class="space-y-2">
              <div 
                v-for="(widgetItem, widgetIndex) in item.items" 
                :key="widgetIndex"
                class="p-3 bg-gradient-to-r from-gray-50 to-gray-100 rounded-lg text-sm text-gray-700 border border-gray-200 hover:border-purple-200 hover:shadow-md transition-all"
              >
                <span class="mr-2">•</span>{{ widgetItem }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  content: {
    type: Array,
    default: () => [
      { 
        type: 'navigation', 
        items: [
          { text: '🏠 Home', link: '#' },
          { text: '👥 About Us', link: '#' },
          { text: '💼 Services', link: '#' },
          { text: '📰 Blog', link: '#' },
          { text: '📞 Contact', link: '#' }
        ] 
      },
      {
        type: 'widget',
        title: '📈 Quick Stats',
        items: [
          '1000+ Happy Clients',
          '50+ Projects Completed',
          '24/7 Support Available',
          '99.9% Uptime Guarantee'
        ]
      },
      {
        type: 'text',
        text: '🚀 We are a leading digital agency specializing in web development, mobile apps, and digital marketing solutions. Our team of experts is dedicated to helping your business grow online.'
      },
      {
        type: 'widget',
        title: '🎯 Popular Services',
        items: [
          'Website Development',
          'Mobile App Design',
          'SEO Optimization',
          'Digital Marketing',
          'E-commerce Solutions'
        ]
      }
    ]
  }
})
</script>

<style scoped>
.left-sidebar {
  width: 100%;
  min-height: auto;
}

.navigation-section ul {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.widget-section {
  margin-top: 1rem;
}
</style>
