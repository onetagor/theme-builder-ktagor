<template>
  <div class="right-sidebar bg-gradient-to-b from-white via-gray-50 to-white border-l border-gray-200 w-full p-6 shadow-lg">
    <div class="space-y-8">
      <div 
        v-for="(item, index) in content" 
        :key="index"
        class="sidebar-item"
      >
        <div v-if="item.type === 'widget'" class="widget-section">
          <div class="bg-gradient-to-r from-purple-50 to-pink-50 rounded-2xl p-6 border border-purple-200 shadow-sm">
            <h3 class="text-2xl font-bold text-gray-900 mb-4 flex items-center">
              <span class="mr-3">{{ item.icon || '📰' }}</span>
              {{ item.title }}
            </h3>
            <div class="grid grid-cols-1 gap-3">
              <div 
                v-for="(widgetItem, widgetIndex) in item.items" 
                :key="widgetIndex"
                class="p-4 bg-white rounded-xl text-gray-700 border border-gray-200 hover:border-purple-300 hover:shadow-md transition-all duration-300 group"
              >
                <div class="flex items-center">
                  <span class="mr-3 text-purple-600">•</span>
                  <span class="font-medium">{{ widgetItem }}</span>
                  <svg class="w-4 h-4 ml-auto text-gray-400 group-hover:text-purple-600 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                  </svg>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <div v-if="item.type === 'text'" class="text-section">
          <div class="bg-gradient-to-r from-blue-50 to-indigo-50 rounded-2xl p-6 border border-blue-200 shadow-sm">
            <div class="prose text-gray-700 leading-relaxed">
              {{ item.text }}
            </div>
          </div>
        </div>
        
        <div v-if="item.type === 'categories'" class="categories-section">
          <div class="bg-gradient-to-r from-green-50 to-emerald-50 rounded-2xl p-6 border border-green-200 shadow-sm">
            <h3 class="text-2xl font-bold text-gray-900 mb-4 flex items-center">
              <span class="mr-3">📁</span>
              Categories
            </h3>
            <div class="space-y-2">
              <a 
                v-for="(category, catIndex) in item.items" 
                :key="catIndex"
                :href="category.link"
                class="group flex items-center p-3 text-gray-700 hover:bg-white hover:text-green-700 rounded-xl transition-all duration-300 border hover:border-green-300 hover:shadow-md"
              >
                <span class="mr-3 text-green-600">{{ category.icon || '🏷️' }}</span>
                <span class="font-medium">{{ category.name }}</span>
                <svg class="w-4 h-4 ml-auto text-gray-400 group-hover:text-green-600 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                </svg>
              </a>
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
        type: 'widget', 
        title: '📰 Latest News',
        items: [
          'New Website Launch Coming Soon',
          'Mobile App Update Released',
          'SEO Tips for Better Rankings',
          'Digital Marketing Trends 2025',
          'Client Success Story Featured'
        ]
      },
      { 
        type: 'categories', 
        items: [
          { name: '💻 Web Development', link: '#' },
          { name: '📱 Mobile Apps', link: '#' },
          { name: '🎨 Design', link: '#' },
          { name: '📈 Marketing', link: '#' },
          { name: '🔧 Support', link: '#' }
        ]
      },
      {
        type: 'widget',
        title: '🎯 Quick Links',
        items: [
          'Download Resources',
          'Schedule Consultation',
          'View Portfolio',
          'Client Testimonials',
          'Contact Support'
        ]
      },
      {
        type: 'widget',
        title: '📊 This Month',
        items: [
          '15 New Projects Started',
          '8 Websites Launched',
          '25 Happy Clients',
          '99.9% Uptime Achieved',
          '50+ Support Tickets Resolved'
        ]
      }
    ]
  }
})
</script>

<style scoped>
.right-sidebar {
  width: 100%;
  min-height: auto;
}

.widget-section {
  margin-bottom: 1rem;
}

.categories-section {
  margin-top: 1rem;
}

.categories-section a {
  display: inline-block;
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>
