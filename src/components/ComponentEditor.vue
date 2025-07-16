<template>
  <div class="component-editor">
    <div class="flex items-center justify-between mb-4">
      <h3 class="text-lg font-semibold text-gray-800">{{ component.type }}</h3>
      <button
        @click="$emit('close')"
        class="text-gray-500 hover:text-gray-700 transition-colors"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </button>
    </div>

    <div class="space-y-4">
      <!-- TopBar Editor -->
      <div v-if="component.type === 'TopBarComponent'" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Columns</label>
          <select v-model="localProps.columns" class="w-full px-3 py-2 border border-gray-300 rounded-md">
            <option value="2">2 Columns</option>
            <option value="3">3 Columns</option>
            <option value="4">4 Columns</option>
          </select>
        </div>
        
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Content</label>
          <div 
            v-for="(item, index) in localProps.content" 
            :key="index"
            class="flex items-center space-x-2 mb-2"
          >
            <input
              v-model="item.text"
              placeholder="Text"
              class="flex-1 px-3 py-2 border border-gray-300 rounded-md text-sm"
            />
            <input
              v-model="item.link"
              placeholder="Link"
              class="flex-1 px-3 py-2 border border-gray-300 rounded-md text-sm"
            />
            <button
              @click="removeContentItem(index)"
              class="text-red-500 hover:text-red-700 p-1"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
              </svg>
            </button>
          </div>
          <button
            @click="addContentItem"
            class="text-sm text-blue-600 hover:text-blue-800 font-medium"
          >
            + Add Item
          </button>
        </div>
      </div>

      <!-- Header Editor -->
      <div v-if="component.type === 'HeaderComponent'" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Title</label>
          <input
            v-model="localProps.title"
            class="w-full px-3 py-2 border border-gray-300 rounded-md"
          />
        </div>
        
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Tagline</label>
          <input
            v-model="localProps.tagline"
            class="w-full px-3 py-2 border border-gray-300 rounded-md"
          />
        </div>
        
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Banner Image URL</label>
          <input
            v-model="localProps.bannerImage"
            placeholder="https://example.com/image.jpg"
            class="w-full px-3 py-2 border border-gray-300 rounded-md"
          />
        </div>
      </div>

      <!-- Logo Editor -->
      <div v-if="component.type === 'LogoComponent'" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Type</label>
          <select v-model="localProps.type" class="w-full px-3 py-2 border border-gray-300 rounded-md">
            <option value="text">Text Logo</option>
            <option value="image">Image Logo</option>
          </select>
        </div>
        
        <div v-if="localProps.type === 'text'">
          <label class="block text-sm font-medium text-gray-700 mb-1">Text</label>
          <input
            v-model="localProps.text"
            class="w-full px-3 py-2 border border-gray-300 rounded-md"
          />
        </div>
        
        <div v-if="localProps.type === 'image'">
          <label class="block text-sm font-medium text-gray-700 mb-1">Image URL</label>
          <input
            v-model="localProps.image"
            placeholder="https://example.com/logo.png"
            class="w-full px-3 py-2 border border-gray-300 rounded-md"
          />
        </div>
      </div>

      <!-- Social Links Editor -->
      <div v-if="component.type === 'SocialLinksComponent'" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Social Links</label>
          <div 
            v-for="(link, index) in localProps.links" 
            :key="index"
            class="flex items-center space-x-2 mb-2"
          >
            <input
              v-model="link.platform"
              placeholder="Platform"
              class="flex-1 px-3 py-2 border border-gray-300 rounded-md text-sm"
            />
            <input
              v-model="link.url"
              placeholder="URL"
              class="flex-1 px-3 py-2 border border-gray-300 rounded-md text-sm"
            />
            <input
              v-model="link.color"
              type="color"
              class="w-10 h-10 border border-gray-300 rounded-md"
            />
            <button
              @click="removeSocialLink(index)"
              class="text-red-500 hover:text-red-700 p-1"
            >
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
              </svg>
            </button>
          </div>
          <button
            @click="addSocialLink"
            class="text-sm text-blue-600 hover:text-blue-800 font-medium"
          >
            + Add Link
          </button>
        </div>
      </div>

      <!-- Footer Editor -->
      <div v-if="component.type === 'FooterComponent'" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">Columns</label>
          <select v-model="localProps.columns" class="w-full px-3 py-2 border border-gray-300 rounded-md">
            <option value="2">2 Columns</option>
            <option value="3">3 Columns</option>
            <option value="4">4 Columns</option>
          </select>
        </div>
        
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Footer Content</label>
          <div 
            v-for="(column, index) in localProps.content" 
            :key="index"
            class="border border-gray-200 rounded-md p-3 mb-3"
          >
            <div class="flex items-center justify-between mb-2">
              <input
                v-model="column.title"
                placeholder="Column Title"
                class="flex-1 px-2 py-1 border border-gray-300 rounded text-sm font-medium"
              />
              <button
                @click="removeFooterColumn(index)"
                class="text-red-500 hover:text-red-700 p-1 ml-2"
              >
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                </svg>
              </button>
            </div>
            <div class="space-y-1">
              <input
                v-for="(item, itemIndex) in column.items"
                :key="itemIndex"
                v-model="column.items[itemIndex]"
                placeholder="Item"
                class="w-full px-2 py-1 border border-gray-300 rounded text-sm"
              />
              <button
                @click="addFooterItem(index)"
                class="text-xs text-blue-600 hover:text-blue-800 font-medium"
              >
                + Add Item
              </button>
            </div>
          </div>
          <button
            @click="addFooterColumn"
            class="text-sm text-blue-600 hover:text-blue-800 font-medium"
          >
            + Add Column
          </button>
        </div>
      </div>

      <!-- Generic Content Editor for Sidebars -->
      <div v-if="component.type === 'LeftSidebarComponent' || component.type === 'RightSidebarComponent'" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-2">Content</label>
          <textarea
            v-model="sidebarContentText"
            @input="updateSidebarContent"
            rows="6"
            class="w-full px-3 py-2 border border-gray-300 rounded-md text-sm"
            placeholder="Add your sidebar content here..."
          ></textarea>
        </div>
      </div>
    </div>

    <!-- Apply Changes Button -->
    <div class="mt-6 pt-4 border-t border-gray-200">
      <button
        @click="applyChanges"
        class="w-full bg-blue-600 text-white py-2 px-4 rounded-md hover:bg-blue-700 transition-colors font-medium"
      >
        Apply Changes
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  component: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['update', 'close'])

// Local copy of props for editing
const localProps = ref({ ...props.component.props })

// Sidebar content helper
const sidebarContentText = ref('')

// Watch for component changes
watch(
  () => props.component,
  (newComponent) => {
    localProps.value = { ...newComponent.props }
    if (newComponent.type === 'LeftSidebarComponent' || newComponent.type === 'RightSidebarComponent') {
      sidebarContentText.value = newComponent.props.content?.[0]?.text || ''
    }
  },
  { immediate: true }
)

// Content management methods
const addContentItem = () => {
  localProps.value.content.push({ text: '', link: '#' })
}

const removeContentItem = (index) => {
  localProps.value.content.splice(index, 1)
}

const addSocialLink = () => {
  localProps.value.links.push({ platform: '', url: '', color: '#000000' })
}

const removeSocialLink = (index) => {
  localProps.value.links.splice(index, 1)
}

const addFooterColumn = () => {
  localProps.value.content.push({ title: '', items: [''] })
}

const removeFooterColumn = (index) => {
  localProps.value.content.splice(index, 1)
}

const addFooterItem = (columnIndex) => {
  localProps.value.content[columnIndex].items.push('')
}

const updateSidebarContent = () => {
  localProps.value.content = [{ type: 'text', text: sidebarContentText.value }]
}

const applyChanges = () => {
  emit('update', {
    ...props.component,
    props: localProps.value
  })
}
</script>

<style scoped>
.component-editor {
  max-height: calc(100vh - 200px);
  overflow-y: auto;
}
</style>
