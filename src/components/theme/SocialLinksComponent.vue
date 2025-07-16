<template>
  <div class="social-links py-6 bg-gray-50">
    <div class="container mx-auto px-4">
      <div class="flex justify-center space-x-6">
        <a 
          v-for="(link, index) in links" 
          :key="index"
          :href="link.url"
          :style="{ color: link.color }"
          class="social-link p-3 rounded-full hover:bg-white hover:shadow-md transition-all duration-200"
          target="_blank"
          rel="noopener noreferrer"
        >
          <div class="flex items-center space-x-2">
            <span class="font-medium">{{ link.platform }}</span>
          </div>
        </a>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  links: {
    type: Array,
    default: () => [
      { platform: '📘 Facebook', url: 'https://facebook.com', color: '#1877F2' },
      { platform: '🐦 Twitter', url: 'https://twitter.com', color: '#1DA1F2' },
      { platform: '📸 Instagram', url: 'https://instagram.com', color: '#E4405F' },
      { platform: '💼 LinkedIn', url: 'https://linkedin.com', color: '#0077B5' },
      { platform: '🎵 TikTok', url: 'https://tiktok.com', color: '#FF0050' },
      { platform: '📺 YouTube', url: 'https://youtube.com', color: '#FF0000' }
    ]
  }
})
</script>

<style scoped>
.social-link {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 48px;
  min-height: 48px;
  border: 2px solid transparent;
}

.social-link:hover {
  border-color: currentColor;
  transform: translateY(-2px);
}
</style>
