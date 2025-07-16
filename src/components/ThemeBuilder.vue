<template>
  <div class="theme-builder">
    <!-- Mode Toggle -->
    <div class="fixed top-6 right-6 z-50">
      <button
        @click="toggleMode"
        :class="[
          'px-6 py-3 rounded-xl font-semibold transition-all duration-300 shadow-lg',
          isEditMode 
            ? 'bg-gradient-to-r from-red-500 to-red-600 text-white hover:from-red-600 hover:to-red-700 hover:scale-105' 
            : 'bg-gradient-to-r from-blue-500 to-blue-600 text-white hover:from-blue-600 hover:to-blue-700 hover:scale-105'
        ]"
      >
        {{ isEditMode ? '🚪 Exit Edit Mode' : '✏️ Edit Mode' }}
      </button>
    </div>

    <!-- Edit Mode - Modern Grid Admin Panel -->
    <div v-if="isEditMode" class="admin-panel">
      <div class="grid-container">
        <!-- Header Bar -->
        <div class="header-bar">
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-4">
              <div class="p-3 rounded-xl">
                <span class="text-white text-2xl">🎨</span>
              </div>
              <div>
                <h1 class="text-2xl font-bold text-gray-800">Theme Builder</h1>
                <p class="text-gray-600">Design your perfect website layout</p>
              </div>
            </div>
            <div class="flex items-center space-x-2 text-sm text-gray-500">
              <span class="bg-green-100 text-green-800 px-3 py-1 rounded-full">
                {{ layoutComponents.length }} Components
              </span>
              <span class="bg-blue-100 text-blue-800 px-3 py-1 rounded-full">
                Edit Mode
              </span>
            </div>
          </div>
        </div>

        <!-- Left Sidebar - Component Toolbox -->
        <div class="toolbox-sidebar">
          <div class="sticky top-4">
            <!-- Quick Actions Grid -->
            <div class="action-grid">
              <button
                @click="previewTheme"
                class="action-btn bg-gradient-to-r from-blue-500 to-blue-600 hover:from-blue-600 hover:to-blue-700"
              >
                <span class="text-2xl mb-2">👁️</span>
                <span class="font-semibold">Preview</span>
              </button>
              <button
                @click="saveTheme"
                class="action-btn bg-gradient-to-r from-green-500 to-green-600 hover:from-green-600 hover:to-green-700"
              >
                <span class="text-2xl mb-2">💾</span>
                <span class="font-semibold">Save</span>
              </button>
              <button
                @click="resetToDefault"
                class="action-btn bg-gradient-to-r from-gray-500 to-gray-600 hover:from-gray-600 hover:to-gray-700"
              >
                <span class="text-2xl mb-2">🔄</span>
                <span class="font-semibold">Reset</span>
              </button>
            </div>

            <!-- Available Components -->
            <div class="components-section">
              <h3 class="section-title">Add Components</h3>
              <div class="components-grid">
                <button
                  v-for="component in availableComponents"
                  :key="component.type"
                  @click="addComponent(component)"
                  class="component-card"
                >
                  <div class="component-icon">{{ component.icon }}</div>
                  <div class="component-info">
                    <div class="component-name">{{ component.name }}</div>
                    <div class="component-desc">{{ component.description }}</div>
                  </div>
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Main Content Area -->
        <div class="main-content">
          <div class="content-container">
            <!-- Empty State -->
            <div v-if="layoutComponents.length === 0" class="empty-state">
              <div class="empty-icon">
                <svg class="w-24 h-24 text-gray-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10"></path>
                </svg>
              </div>
              <h3 class="text-2xl font-bold text-gray-700 mb-4">Start Building Your Theme</h3>
              <p class="text-gray-500 text-lg mb-6">Choose components from the sidebar to create your perfect layout</p>
              <div class="flex items-center justify-center space-x-4">
                <span class="text-sm text-gray-400">👈 Click components to add them</span>
              </div>
            </div>

            <!-- Components Grid -->
            <div v-else class="components-workspace">
              <div
                v-for="(element, index) in layoutComponents"
                :key="element.id"
                class="component-item"
              >
                <!-- Component Controls -->
                <div class="component-controls">
                  <div class="control-badge">
                    {{ getComponentName(element.type) }}
                  </div>
                  <div class="control-buttons">
                    <button
                      @click="moveUp(index)"
                      :disabled="index === 0"
                      class="control-btn control-btn-move"
                      title="Move Up"
                    >
                      ↑
                    </button>
                    <button
                      @click="moveDown(index)"
                      :disabled="index === layoutComponents.length - 1"
                      class="control-btn control-btn-move"
                      title="Move Down"
                    >
                      ↓
                    </button>
                    <button
                      @click="removeComponent(index)"
                      class="control-btn control-btn-delete"
                      title="Delete"
                    >
                      🗑️
                    </button>
                  </div>
                </div>

                <!-- Component Preview -->
                <div class="component-preview">
                  <component
                    :is="componentRegistry[element.type]"
                    v-bind="element.props"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Public View Mode -->
    <div v-else class="public-view">
      <!-- Preview Mode Indicator -->
      <div v-if="isPreviewMode" class="preview-indicator fixed top-4 left-4 bg-blue-600 text-white px-4 py-2 rounded-lg shadow-lg z-40">
        <span class="text-sm">👁️ Preview Mode - Public View</span>
      </div>

      <!-- Render Layout -->
      <div class="min-h-screen bg-white">
        <div v-if="layoutComponents.length === 0" class="text-center py-20">
          <h2 class="text-2xl font-bold text-gray-800 mb-4">No Components Found</h2>
          <p class="text-gray-600">Click "Edit Mode" to add components</p>
        </div>
        
        <!-- Render components in order -->
        <component
          v-for="element in layoutComponents"
          :key="element.id"
          :is="componentRegistry[element.type]"
          v-bind="element.props"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import TopBarComponent from './theme/TopBarComponent.vue'
import HeaderComponent from './theme/HeaderComponent.vue'
import LogoComponent from './theme/LogoComponent.vue'
import SocialLinksComponent from './theme/SocialLinksComponent.vue'
import LeftSidebarComponent from './theme/LeftSidebarComponent.vue'
import RightSidebarComponent from './theme/RightSidebarComponent.vue'
import FooterComponent from './theme/FooterComponent.vue'

// State
const isEditMode = ref(false)
const isPreviewMode = ref(false)
const layoutComponents = ref([])

// Component registry for dynamic rendering
const componentRegistry = {
  TopBarComponent,
  HeaderComponent,
  LogoComponent,
  SocialLinksComponent,
  LeftSidebarComponent,
  RightSidebarComponent,
  FooterComponent
}

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
    description: 'Hero section with title and call-to-action',
    icon: '🏠'
  },
  {
    type: 'LogoComponent',
    name: 'Logo',
    description: 'Brand logo section',
    icon: '🎭'
  },
  {
    type: 'SocialLinksComponent',
    name: 'Social Links',
    description: 'Social media platform links',
    icon: '📱'
  },
  {
    type: 'LeftSidebarComponent',
    name: 'Left Sidebar',
    description: 'Navigation sidebar on the left',
    icon: '◀️'
  },
  {
    type: 'RightSidebarComponent',
    name: 'Right Sidebar',
    description: 'Content sidebar on the right',
    icon: '▶️'
  },
  {
    type: 'FooterComponent',
    name: 'Footer',
    description: 'Bottom section with links and info',
    icon: '📄'
  }
]

// Default theme with rich content
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
          logoText: '🚀 TechCorp Solutions',
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
    id: 4,
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

// Toggle edit mode
const toggleMode = () => {
  isEditMode.value = !isEditMode.value
  isPreviewMode.value = false
}

// Preview theme
const previewTheme = () => {
  isEditMode.value = false
  isPreviewMode.value = true
  
  // Show preview notification
  setTimeout(() => {
    alert('👁️ Preview Mode Activated! This is how visitors will see your website.')
  }, 100)
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

// Default props for components
const getDefaultProps = (type) => {
  const defaults = {
    TopBarComponent: {
      columns: 4,
      sections: [
        {
          type: 'logo',
          logoType: 'text',
          logoText: '🚀 TechCorp Solutions',
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
      title: 'Welcome to Our Website',
      tagline: 'We help businesses grow with innovative solutions and exceptional service. Join thousands of satisfied customers.',
      bannerImage: ''
    },
    LogoComponent: {
      type: 'text',
      text: '🚀 TechCorp Solutions',
      image: ''
    },
    SocialLinksComponent: {
      links: [
        { platform: '📘 Facebook', url: 'https://facebook.com', color: '#1877F2' },
        { platform: '🐦 Twitter', url: 'https://twitter.com', color: '#1DA1F2' },
        { platform: '📸 Instagram', url: 'https://instagram.com', color: '#E4405F' },
        { platform: '💼 LinkedIn', url: 'https://linkedin.com', color: '#0077B5' }
      ]
    },
    LeftSidebarComponent: {
      content: [
        { 
          type: 'navigation', 
          items: [
            { text: '🏠 Home', link: '#' },
            { text: '👥 About Us', link: '#' },
            { text: '💼 Services', link: '#' },
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
    },
    RightSidebarComponent: {
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
    },
    FooterComponent: {
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

// Remove component
const removeComponent = (index) => {
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
  console.log('🚀 Component mounted!')
  console.log('📦 Default theme:', defaultTheme)
  
  // Clear localStorage for fresh start
  localStorage.removeItem('theme-builder-data')
  
  const saved = localStorage.getItem('theme-builder-data')
  console.log('💾 Saved data:', saved)
  
  if (saved) {
    const themeData = JSON.parse(saved)
    console.log('📊 Parsed theme data:', themeData)
    layoutComponents.value = themeData.layout || [...defaultTheme]
  } else {
    // Load default theme
    console.log('🔄 Loading default theme')
    layoutComponents.value = [...defaultTheme]
  }
  
  console.log('🎨 Final layout components:', layoutComponents.value)
})
</script>

<style scoped>
.theme-builder {
  min-height: 100vh;
  position: relative;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  width: 100%;
  max-width: 100%;
  margin: 0;
  padding: 0;
}

/* Admin Panel Grid Layout */
.admin-panel {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
  width: 100%;
  overflow: hidden;
}

.grid-container {
  display: grid;
  grid-template-columns: 320px 1fr;
  grid-template-rows: 80px 1fr;
  grid-template-areas:
    "header header"
    "sidebar content";
  min-height: 100vh;
  gap: 0;
  width: 100%;
}

/* Header Bar */
.header-bar {
  grid-area: header;
  background: white;
  border-bottom: 1px solid #e5e7eb;
  padding: 0 2rem;
  display: flex;
  align-items: center;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

/* Toolbox Sidebar */
.toolbox-sidebar {
  grid-area: sidebar;
  background: white;
  border-right: 1px solid #e5e7eb;
  padding: 1.5rem;
  overflow-y: auto;
  box-shadow: 2px 0 4px rgba(0, 0, 0, 0.05);
}

/* Main Content */
.main-content {
  grid-area: content;
  background: #f8fafc;
  padding: 2rem;
  overflow-y: auto;
}

.content-container {
  max-width: 1200px;
  margin: 0 auto;
}

/* Action Grid */
.action-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-bottom: 2rem;
}

.action-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  border: none;
  border-radius: 12px;
  color: white;
  font-weight: 600;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  min-height: 80px;
}

.action-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

/* Components Section */
.components-section {
  background: #f8fafc;
  border-radius: 12px;
  padding: 1.5rem;
  border: 1px solid #e5e7eb;
}

.section-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #374151;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.section-title::before {
  content: "🧩";
  font-size: 1.25rem;
}

.components-grid {
  display: grid;
  gap: 0.75rem;
}

.component-card {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem;
  background: white;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  transition: all 0.3s ease;
  cursor: pointer;
  text-align: left;
}

.component-card:hover {
  border-color: #3b82f6;
  background: #f0f9ff;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(59, 130, 246, 0.1);
}

.component-icon {
  font-size: 1.5rem;
  width: 2.5rem;
  height: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #f3f4f6;
  border-radius: 8px;
  flex-shrink: 0;
}

.component-info {
  flex: 1;
}

.component-name {
  font-weight: 600;
  color: #111827;
  margin-bottom: 0.25rem;
}

.component-desc {
  font-size: 0.875rem;
  color: #6b7280;
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  background: white;
  border-radius: 12px;
  border: 2px dashed #e5e7eb;
  margin: 2rem 0;
}

.empty-icon {
  margin-bottom: 1.5rem;
}

/* Components Workspace */
.components-workspace {
  display: grid;
  gap: 1.5rem;
}

.component-item {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  border: 1px solid #e5e7eb;
  transition: all 0.3s ease;
  position: relative;
}

.component-item:hover {
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

/* Component Controls */
.component-controls {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.75rem 1rem;
  background: #f8fafc;
  border-bottom: 1px solid #e5e7eb;
}

.control-badge {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  font-size: 0.875rem;
  font-weight: 600;
}

.control-buttons {
  display: flex;
  gap: 0.5rem;
}

.control-btn {
  padding: 0.5rem;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  background: white;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 0.875rem;
  min-width: 2rem;
  height: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.control-btn:hover {
  background: #f3f4f6;
  transform: scale(1.05);
}

.control-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
}

.control-btn-move {
  color: #3b82f6;
  border-color: #3b82f6;
}

.control-btn-move:hover {
  background: #dbeafe;
}

.control-btn-delete {
  color: #ef4444;
  border-color: #ef4444;
}

.control-btn-delete:hover {
  background: #fee2e2;
}

/* Component Preview */
.component-preview {
  position: relative;
  overflow: hidden;
}

/* Public View */
.public-view {
  background: white;
  min-height: 100vh;
  width: 100%;
  margin: 0;
  padding: 0;
}

.preview-indicator {
  z-index: 1000;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}

/* Responsive Design */
@media (max-width: 1024px) {
  .grid-container {
    grid-template-columns: 280px 1fr;
  }
  
  .action-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .grid-container {
    grid-template-columns: 1fr;
    grid-template-rows: 80px auto 1fr;
    grid-template-areas:
      "header"
      "sidebar"
      "content";
  }
  
  .toolbox-sidebar {
    max-height: 300px;
    overflow-y: auto;
  }
  
  .action-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .main-content {
    padding: 1rem;
  }
}

@media (max-width: 480px) {
  .action-grid {
    grid-template-columns: 1fr;
  }
  
  .header-bar {
    padding: 0 1rem;
  }
  
  .header-bar h1 {
    font-size: 1.25rem;
  }
  
  .toolbox-sidebar {
    padding: 1rem;
  }
}

/* Button Enhancements */
button {
  transition: all 0.2s ease;
}

button:hover {
  transform: translateY(-1px);
}

button:active {
  transform: translateY(0);
}

button:disabled {
  cursor: not-allowed;
  transform: none;
}

/* Component Wrapper */
.component-wrapper {
  position: relative;
  border-radius: 8px;
  background: #ffffff;
  width: 100%;
}

.component-wrapper:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}
</style>
