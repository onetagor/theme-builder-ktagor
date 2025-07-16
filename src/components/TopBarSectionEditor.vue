<template>
  <div class="topbar-section-editor">
    <div class="bg-white p-4 rounded-lg border border-gray-200">
      <div class="flex items-center justify-between mb-4">
        <h3 class="text-lg font-semibold text-gray-800">📊 TopBar Layout</h3>
        <div class="flex items-center space-x-2">
          <label class="text-sm text-gray-600">Columns:</label>
          <select 
            v-model="localProps.columns" 
            class="px-2 py-1 border border-gray-300 rounded text-sm"
          >
            <option value="2">2 Columns</option>
            <option value="3">3 Columns</option>
            <option value="4">4 Columns</option>
          </select>
        </div>
      </div>

      <!-- Section Management -->
      <div class="mb-4">
        <h4 class="font-medium text-gray-700 mb-2">📦 Current Sections:</h4>
        <div class="space-y-2">
          <div 
            v-for="(section, index) in localProps.sections" 
            :key="index"
            class="flex items-center justify-between p-3 bg-gray-50 rounded border"
          >
            <div class="flex items-center space-x-3">
              <span class="text-lg">{{ getSectionIcon(section.type) }}</span>
              <div>
                <div class="font-medium text-gray-800">{{ getSectionName(section.type) }}</div>
                <div class="text-sm text-gray-600">{{ getSectionDescription(section) }}</div>
              </div>
            </div>
            
            <div class="flex items-center space-x-2">
              <!-- Move Up -->
              <button
                @click="moveSection(index, -1)"
                :disabled="index === 0"
                class="p-1 bg-blue-500 text-white rounded hover:bg-blue-600 text-xs disabled:opacity-50"
              >
                ↑
              </button>
              
              <!-- Move Down -->
              <button
                @click="moveSection(index, 1)"
                :disabled="index === localProps.sections.length - 1"
                class="p-1 bg-blue-500 text-white rounded hover:bg-blue-600 text-xs disabled:opacity-50"
              >
                ↓
              </button>
              
              <!-- Remove -->
              <button
                @click="removeSection(index)"
                class="p-1 bg-red-500 text-white rounded hover:bg-red-600 text-xs"
              >
                🗑️
              </button>
            </div>
          </div>
        </div>

        <!-- Add New Section -->
        <div class="mt-4">
          <h4 class="font-medium text-gray-700 mb-2">➕ Add Section:</h4>
          <div class="grid grid-cols-2 gap-2">
            <button
              @click="addSection('logo')"
              class="flex items-center space-x-2 p-2 bg-blue-50 border border-blue-200 rounded hover:bg-blue-100 text-sm"
            >
              <span>🎭</span>
              <span>Logo</span>
            </button>
            
            <button
              @click="addSection('menu')"
              class="flex items-center space-x-2 p-2 bg-green-50 border border-green-200 rounded hover:bg-green-100 text-sm"
            >
              <span>📋</span>
              <span>Menu</span>
            </button>
            
            <button
              @click="addSection('contact')"
              class="flex items-center space-x-2 p-2 bg-yellow-50 border border-yellow-200 rounded hover:bg-yellow-100 text-sm"
            >
              <span>📞</span>
              <span>Contact</span>
            </button>
            
            <button
              @click="addSection('social')"
              class="flex items-center space-x-2 p-2 bg-purple-50 border border-purple-200 rounded hover:bg-purple-100 text-sm"
            >
              <span>📱</span>
              <span>Social</span>
            </button>
          </div>
        </div>
      </div>

      <!-- Section Content Editor -->
      <div v-if="selectedSection" class="mt-6 p-4 bg-gray-50 rounded-lg">
        <h4 class="font-medium text-gray-700 mb-3">✏️ Edit {{ getSectionName(selectedSection.type) }}</h4>
        
        <!-- Logo Section Editor -->
        <div v-if="selectedSection.type === 'logo'" class="space-y-3">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Logo Type:</label>
            <select 
              v-model="selectedSection.logoType" 
              class="w-full px-3 py-2 border border-gray-300 rounded-md"
            >
              <option value="text">Text Logo</option>
              <option value="image">Image Logo</option>
            </select>
          </div>
          
          <div v-if="selectedSection.logoType === 'text'">
            <label class="block text-sm font-medium text-gray-700 mb-1">Logo Text:</label>
            <input 
              v-model="selectedSection.logoText" 
              type="text" 
              class="w-full px-3 py-2 border border-gray-300 rounded-md"
              placeholder="Your Brand Name"
            />
          </div>
          
          <div v-if="selectedSection.logoType === 'image'">
            <label class="block text-sm font-medium text-gray-700 mb-1">Logo Image URL:</label>
            <input 
              v-model="selectedSection.logoImage" 
              type="url" 
              class="w-full px-3 py-2 border border-gray-300 rounded-md"
              placeholder="https://example.com/logo.png"
            />
          </div>
        </div>

        <!-- Menu Section Editor -->
        <div v-if="selectedSection.type === 'menu'" class="space-y-3">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-2">Menu Items:</label>
            <div class="space-y-2">
              <div 
                v-for="(item, index) in selectedSection.menuItems" 
                :key="index"
                class="flex items-center space-x-2"
              >
                <input 
                  v-model="item.text" 
                  type="text" 
                  class="flex-1 px-3 py-2 border border-gray-300 rounded-md"
                  placeholder="Menu Text"
                />
                <input 
                  v-model="item.link" 
                  type="text" 
                  class="flex-1 px-3 py-2 border border-gray-300 rounded-md"
                  placeholder="Link URL"
                />
                <button
                  @click="removeMenuItem(index)"
                  class="p-2 bg-red-500 text-white rounded hover:bg-red-600 text-sm"
                >
                  ❌
                </button>
              </div>
            </div>
            <button
              @click="addMenuItem()"
              class="mt-2 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 text-sm"
            >
              + Add Menu Item
            </button>
          </div>
        </div>

        <!-- Contact Section Editor -->
        <div v-if="selectedSection.type === 'contact'" class="space-y-3">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">Phone Number:</label>
            <input 
              v-model="selectedSection.phoneNumber" 
              type="tel" 
              class="w-full px-3 py-2 border border-gray-300 rounded-md"
              placeholder="+1 (555) 123-4567"
            />
          </div>
        </div>

        <!-- Social Links Section Editor -->
        <div v-if="selectedSection.type === 'social'" class="space-y-3">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-2">Social Links:</label>
            <div class="space-y-2">
              <div 
                v-for="(link, index) in selectedSection.socialLinks" 
                :key="index"
                class="flex items-center space-x-2"
              >
                <input 
                  v-model="link.platform" 
                  type="text" 
                  class="flex-1 px-3 py-2 border border-gray-300 rounded-md"
                  placeholder="Platform (e.g., Facebook)"
                />
                <input 
                  v-model="link.url" 
                  type="url" 
                  class="flex-1 px-3 py-2 border border-gray-300 rounded-md"
                  placeholder="https://facebook.com/yourpage"
                />
                <button
                  @click="removeSocialLink(index)"
                  class="p-2 bg-red-500 text-white rounded hover:bg-red-600 text-sm"
                >
                  ❌
                </button>
              </div>
            </div>
            <button
              @click="addSocialLink()"
              class="mt-2 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 text-sm"
            >
              + Add Social Link
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  componentProps: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['update:componentProps'])

const localProps = ref({ ...props.componentProps })
const selectedSection = ref(null)

// Watch for changes and emit
watch(localProps, (newProps) => {
  emit('update:componentProps', newProps)
}, { deep: true })

// Section management
const getSectionIcon = (type) => {
  const icons = {
    logo: '🎭',
    menu: '📋',
    contact: '📞',
    social: '📱'
  }
  return icons[type] || '❓'
}

const getSectionName = (type) => {
  const names = {
    logo: 'Logo',
    menu: 'Navigation Menu',
    contact: 'Contact Info',
    social: 'Social Links'
  }
  return names[type] || 'Unknown'
}

const getSectionDescription = (section) => {
  switch (section.type) {
    case 'logo':
      return section.logoType === 'text' ? `Text: "${section.logoText}"` : 'Image Logo'
    case 'menu':
      return `${section.menuItems?.length || 0} menu items`
    case 'contact':
      return section.phoneNumber || 'No phone number'
    case 'social':
      return `${section.socialLinks?.length || 0} social links`
    default:
      return 'Unknown section'
  }
}

const moveSection = (index, direction) => {
  const newIndex = index + direction
  if (newIndex >= 0 && newIndex < localProps.value.sections.length) {
    const sections = [...localProps.value.sections]
    const temp = sections[index]
    sections[index] = sections[newIndex]
    sections[newIndex] = temp
    localProps.value.sections = sections
  }
}

const removeSection = (index) => {
  if (confirm('Are you sure you want to remove this section?')) {
    localProps.value.sections.splice(index, 1)
    if (selectedSection.value === localProps.value.sections[index]) {
      selectedSection.value = null
    }
  }
}

const addSection = (type) => {
  const newSection = {
    type,
    ...getDefaultSectionProps(type)
  }
  localProps.value.sections.push(newSection)
  selectedSection.value = newSection
}

const getDefaultSectionProps = (type) => {
  switch (type) {
    case 'logo':
      return {
        logoType: 'text',
        logoText: 'Your Brand',
        logoImage: ''
      }
    case 'menu':
      return {
        menuItems: [
          { text: 'Home', link: '#' },
          { text: 'About', link: '#' }
        ]
      }
    case 'contact':
      return {
        phoneNumber: '+1 (555) 123-4567'
      }
    case 'social':
      return {
        socialLinks: [
          { platform: 'Facebook', url: 'https://facebook.com' },
          { platform: 'Twitter', url: 'https://twitter.com' }
        ]
      }
    default:
      return {}
  }
}

// Menu item management
const addMenuItem = () => {
  if (!selectedSection.value.menuItems) {
    selectedSection.value.menuItems = []
  }
  selectedSection.value.menuItems.push({ text: 'New Item', link: '#' })
}

const removeMenuItem = (index) => {
  selectedSection.value.menuItems.splice(index, 1)
}

// Social link management
const addSocialLink = () => {
  if (!selectedSection.value.socialLinks) {
    selectedSection.value.socialLinks = []
  }
  selectedSection.value.socialLinks.push({ platform: 'Platform', url: 'https://' })
}

const removeSocialLink = (index) => {
  selectedSection.value.socialLinks.splice(index, 1)
}
</script>

<style scoped>
.topbar-section-editor {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.topbar-section-editor input,
.topbar-section-editor select {
  transition: border-color 0.2s ease;
}

.topbar-section-editor input:focus,
.topbar-section-editor select:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.topbar-section-editor button {
  transition: all 0.2s ease;
}

.topbar-section-editor button:hover {
  transform: translateY(-1px);
}

.topbar-section-editor button:disabled {
  cursor: not-allowed;
  transform: none;
}
</style>
