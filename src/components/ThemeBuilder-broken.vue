<template>
  <div class="theme-builder">
    <!-- Mode Toggle -->
    <div class="fixed top-4 right-4 z-50">
      <button
        @click="toggleMode"
        :class="[
          'px-4 py-2 rounded-lg font-medium transition-colors',
          isEditMode 
            ? 'bg-red-600 text-white hover:bg-red-700' 
            : 'bg-blue-600 text-white hover:bg-blue-700'
        ]"
      >
        {{ isEditMode ? 'Exit Edit Mode' : 'Edit Mode' }}
      </button>
    </div>

    <!-- Edit Mode - Admin Panel -->
    <div v-if="isEditMode" class="admin-panel">
      <div class="flex h-screen bg-gray-100">
        <!-- Left Sidebar - Component Toolbox -->
        <div class="w-80 bg-white border-r border-gray-300 p-4 overflow-y-auto">
          <h2 class="text-xl font-bold mb-4 text-gray-800">🎨 Admin Panel</h2>
          
          <!-- Theme Actions -->
          <div class="mb-6">
            <button
              @click="previewTheme"
              class="w-full bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition-colors mb-2"
            >
              👁️ Preview Theme
            </button>
            <button
              @click="saveTheme"
              class="w-full bg-green-600 text-white px-4 py-2 rounded-lg hover:bg-green-700 transition-colors mb-2"
            >
              💾 Save Theme
            </button>
            <button
              @click="resetToDefault"
              class="w-full bg-gray-600 text-white px-4 py-2 rounded-lg hover:bg-gray-700 transition-colors"
            >
              🔄 Reset to Default
            </button>
          </div>

          <!-- Available Components -->
          <h3 class="text-lg font-semibold mb-3 text-gray-700">Add Components:</h3>
          <div class="space-y-2">
            <button
              v-for="component in availableComponents"
              :key="component.type"
              @click="addComponent(component)"
              class="w-full text-left bg-gray-50 p-3 rounded-lg border border-gray-200 hover:bg-gray-100 transition-colors"
            >
              <div class="flex items-center space-x-2">
                <span class="text-lg">{{ component.icon }}</span>
                <div>
                  <div class="font-medium text-gray-800">{{ component.name }}</div>
                  <div class="text-sm text-gray-600">{{ component.description }}</div>
                </div>
              </div>
            </button>
          </div>
        </div>

        <!-- Main Canvas Area -->
        <div class="flex-1 flex flex-col">
          <!-- Top Toolbar -->
          <div class="bg-white border-b border-gray-300 p-4">
            <div class="flex items-center justify-between">
              <h1 class="text-2xl font-bold text-gray-800">Theme Editor</h1>
              <div class="text-sm text-gray-600">
                {{ layoutComponents.length }} components
              </div>
            </div>
          </div>

          <!-- Canvas -->
          <div class="flex-1 bg-gray-50 p-6 overflow-y-auto">
            <div class="bg-white rounded-lg shadow-sm border border-gray-200 min-h-full">
              <!-- Empty State -->
              <div v-if="layoutComponents.length === 0" class="text-center py-20">
                <div class="text-gray-400 mb-4">
                  <svg class="w-16 h-16 mx-auto" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6"></path>
                  </svg>
                </div>
                <p class="text-gray-500 text-lg mb-4">No components added yet</p>
                <p class="text-gray-400">Click components from the left panel to add them</p>
              </div>

              <!-- Components List -->
              <div class="space-y-2 p-4">
                <div
                  v-for="(element, index) in layoutComponents"
                  :key="element.id"
                  class="component-wrapper relative group border-2 border-dashed border-gray-200 rounded-lg p-4 hover:border-blue-400 transition-colors"
                >
                  <!-- Component Controls -->
                  <div class="absolute top-2 right-2 opacity-0 group-hover:opacity-100 transition-opacity">
                    <div class="flex space-x-1">
                      <button
                        @click="moveUp(index)"
                        :disabled="index === 0"
                        class="p-1 bg-blue-500 text-white rounded hover:bg-blue-600 disabled:opacity-50 disabled:cursor-not-allowed text-xs"
                      >
                        ↑
                      </button>
                      <button
                        @click="moveDown(index)"
                        :disabled="index === layoutComponents.length - 1"
                        class="p-1 bg-blue-500 text-white rounded hover:bg-blue-600 disabled:opacity-50 disabled:cursor-not-allowed text-xs"
                      >
                        ↓
                      </button>
                      <button
                        @click="deleteComponent(index)"
                        class="p-1 bg-red-500 text-white rounded hover:bg-red-600 text-xs"
                      >
                        ✕
                      </button>
                    </div>
                  </div>

                  <!-- Component Label -->
                  <div class="absolute top-2 left-2 opacity-0 group-hover:opacity-100 transition-opacity">
                    <span class="text-xs bg-gray-800 text-white px-2 py-1 rounded">
                      {{ getComponentName(element.type) }}
                    </span>
                  </div>

                  <!-- Component Content -->
                  <component
                    :is="element.type"
                    v-bind="element.props"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- View Mode - Public Website -->
    <div v-else class="public-website">
      <!-- Preview Mode Indicator -->
      <div class="preview-indicator fixed top-4 left-4 bg-blue-600 text-white px-4 py-2 rounded-lg shadow-lg z-40">
        <span class="text-sm">👁️ Preview Mode - Public View</span>
      </div>
      
      <div class="min-h-screen bg-white">
        <!-- Render components in order -->
        <component
          v-for="element in layoutComponents"
          :key="element.id"
          :is="element.type"
          v-bind="element.props"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// Import theme components
import TopBarComponent from './theme/TopBarComponent.vue'
import HeaderComponent from './theme/HeaderComponent.vue'
import LogoComponent from './theme/LogoComponent.vue'
import SocialLinksComponent from './theme/SocialLinksComponent.vue'
import FooterComponent from './theme/FooterComponent.vue'

// State
const isEditMode = ref(false)
const layoutComponents = ref([])

// Available components
const availableComponents = [
  {
    type: 'TopBarComponent',
    name: 'Top Bar',
    description: 'Navigation bar with logo, menu, contact & social links',
    icon: '🧭'
  },
  {
    type: 'HeaderComponent',
    name: 'Header',
    description: 'Hero section',
    icon: '🏠'
  },
  {
    type: 'LogoComponent',
    name: 'Logo',
    description: 'Brand logo',
    icon: '🎭'
  },
  {
    type: 'SocialLinksComponent',
    name: 'Social Links',
    description: 'Social media',
    icon: '📱'
  },
  {
    type: 'FooterComponent',
    name: 'Footer',
    description: 'Footer section',
    icon: '🦶'
  }
]

// Default theme layout
const defaultTheme = [
  {
    id: 1,
    type: 'TopBarComponent',
    props: {
      columns: 4,
      sections: [
        {
          type: 'logo',
          logoType: 'text',
          logoText: 'Your Brand',
          logoImage: ''
        },
        {
          type: 'menu',
          menuItems: [
            { text: 'Home', link: '#' },
            { text: 'About', link: '#' },
            { text: 'Services', link: '#' },
            { text: 'Contact', link: '#' }
          ]
        },
        {
          type: 'contact',
          phoneNumber: '+1 (555) 123-4567'
        },
        {
          type: 'social',
          socialLinks: [
            { platform: 'Facebook', url: 'https://facebook.com' },
            { platform: 'Twitter', url: 'https://twitter.com' },
            { platform: 'Instagram', url: 'https://instagram.com' }
          ]
        }
      ]
    }
  },
  {
    id: 2,
    type: 'HeaderComponent',
    props: {
      title: 'Welcome to Our Amazing Website',
      tagline: 'We help businesses grow with innovative solutions and exceptional service. Join thousands of satisfied customers.',
      bannerImage: ''
    }
  },
  {
    id: 3,
    type: 'LogoComponent',
    props: {
      type: 'text',
      text: '🚀 TechCorp Solutions',
      image: ''
    }
  },
  {
    id: 4,
    type: 'LeftSidebarComponent',
    props: {
      content: [
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
        }
      ]
    }
  },
  {
    id: 5,
    type: 'RightSidebarComponent',
    props: {
      content: [
        { 
          type: 'widget', 
          title: '📰 Latest News',
          items: [
            'New Website Launch Coming Soon',
            'Mobile App Update Released',
            'SEO Tips for Better Rankings',
            'Digital Marketing Trends 2025'
          ]
        },
        { 
          type: 'categories', 
          items: [
            { name: '💻 Web Development', link: '#' },
            { name: '📱 Mobile Apps', link: '#' },
            { name: '🎨 Design', link: '#' },
            { name: '📈 Marketing', link: '#' }
          ]
        }
      ]
    }
  },
  {
    id: 6,
    type: 'SocialLinksComponent',
    props: {
      links: [
        { platform: '📘 Facebook', url: 'https://facebook.com', color: '#1877F2' },
        { platform: '🐦 Twitter', url: 'https://twitter.com', color: '#1DA1F2' },
        { platform: '📸 Instagram', url: 'https://instagram.com', color: '#E4405F' },
        { platform: '💼 LinkedIn', url: 'https://linkedin.com', color: '#0077B5' }
      ]
    }
  },
  {
    id: 7,
    type: 'FooterComponent',
    props: {
      columns: 4,
      content: [
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
  },
  {
    id: 4,
    type: 'SocialLinksComponent',
    props: {
      links: [
        { platform: '📘 Facebook', url: 'https://facebook.com', color: '#1877F2' },
        { platform: '🐦 Twitter', url: 'https://twitter.com', color: '#1DA1F2' },
        { platform: '📸 Instagram', url: 'https://instagram.com', color: '#E4405F' },
        { platform: '💼 LinkedIn', url: 'https://linkedin.com', color: '#0077B5' }
      ]
    }
  },
  {
    id: 5,
    type: 'FooterComponent',
    props: {
      columns: 4,
      content: [
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
  }
]

// Toggle between edit and view mode
const toggleMode = () => {
  isEditMode.value = !isEditMode.value
}

// Preview theme - exit edit mode to show public view
const previewTheme = () => {
  isEditMode.value = false
  // Show temporary message
  const message = document.createElement('div')
  message.innerHTML = '👁️ Preview Mode - This is how visitors see your website'
  message.className = 'fixed top-20 left-1/2 transform -translate-x-1/2 bg-blue-600 text-white px-6 py-3 rounded-lg shadow-lg z-50'
  document.body.appendChild(message)
  
  // Remove message after 3 seconds
  setTimeout(() => {
    document.body.removeChild(message)
  }, 3000)
}

// Add component
const addComponent = (componentData) => {
  const newComponent = {
    id: Date.now(),
    type: componentData.type,
    props: getDefaultProps(componentData.type)
  }
  layoutComponents.value.push(newComponent)
}

// Get component name
const getComponentName = (type) => {
  const component = availableComponents.find(c => c.type === type)
  return component ? component.name : type
}

// Default props
const getDefaultProps = (type) => {
  const defaults = {
    TopBarComponent: {
      columns: 4,
      sections: [
        {
          type: 'logo',
          logoType: 'text',
          logoText: 'Your Brand',
          logoImage: ''
        },
        {
          type: 'menu',
          menuItems: [
            { text: 'Home', link: '#' },
            { text: 'About', link: '#' },
            { text: 'Contact', link: '#' }
          ]
        },
        {
          type: 'contact',
          phoneNumber: '+1 (555) 123-4567'
        },
        {
          type: 'social',
          socialLinks: [
            { platform: 'Facebook', url: 'https://facebook.com' },
            { platform: 'Twitter', url: 'https://twitter.com' },
            { platform: 'Instagram', url: 'https://instagram.com' }
          ]
        }
      ]
    },
    HeaderComponent: {
      title: 'New Header',
      tagline: 'Add your tagline here',
      bannerImage: ''
    },
    LogoComponent: {
      type: 'text',
      text: 'Your Brand',
      image: ''
    },
    SocialLinksComponent: {
      links: [
        { platform: 'Facebook', url: '#', color: '#1877F2' },
        { platform: 'Twitter', url: '#', color: '#1DA1F2' }
      ]
    },
    FooterComponent: {
      columns: 3,
      content: [
        { title: 'Company', items: ['About', 'Services'] },
        { title: 'Resources', items: ['Blog', 'Help'] },
        { title: 'Legal', items: ['Privacy', 'Terms'] }
      ]
    }
  }
  return defaults[type] || {}
}

// Move component up
const moveUp = (index) => {
  if (index > 0) {
    const item = layoutComponents.value.splice(index, 1)[0]
    layoutComponents.value.splice(index - 1, 0, item)
  }
}

// Move component down
const moveDown = (index) => {
  if (index < layoutComponents.value.length - 1) {
    const item = layoutComponents.value.splice(index, 1)[0]
    layoutComponents.value.splice(index + 1, 0, item)
  }
}

// Delete component
const deleteComponent = (index) => {
  if (confirm('Are you sure you want to delete this component?')) {
    layoutComponents.value.splice(index, 1)
  }
}

// Save theme
const saveTheme = () => {
  const themeData = {
    layout: layoutComponents.value,
    timestamp: new Date().toISOString()
  }
  localStorage.setItem('theme-builder-data', JSON.stringify(themeData))
  alert('✅ Theme saved successfully!')
}

// Reset to default
const resetToDefault = () => {
  if (confirm('Are you sure you want to reset to the default theme? This will lose all changes.')) {
    layoutComponents.value = [...defaultTheme]
    saveTheme()
  }
}

// Load theme on mount
onMounted(() => {
  const saved = localStorage.getItem('theme-builder-data')
  if (saved) {
    const themeData = JSON.parse(saved)
    layoutComponents.value = themeData.layout || [...defaultTheme]
  } else {
    // Load default theme
    layoutComponents.value = [...defaultTheme]
  }
})
</script>

<style scoped>
.component-wrapper {
  position: relative;
}

.component-wrapper:hover {
  background-color: rgba(59, 130, 246, 0.05);
}

.admin-panel {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  background: white;
}

.public-website {
  min-height: 100vh;
}
</style>
