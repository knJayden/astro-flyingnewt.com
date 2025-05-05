<template>
    <div class="gallery">
      <!-- Gallery Categories -->
      <div class="flex justify-center mb-8">
        <div class="inline-flex rounded-md shadow-sm" role="group">
          <button 
            v-for="category in categories" 
            :key="category.id"
            @click="activeCategory = category.id"
            :class="[
              'px-4 py-2 text-sm font-medium',
              activeCategory === category.id 
                ? 'bg-rose-600 text-white' 
                : 'bg-white text-gray-700 hover:bg-gray-50',
              category.id === 'all' ? 'rounded-l-lg' : '',
              category.id === categories[categories.length - 1].id ? 'rounded-r-lg' : ''
            ]"
          >
            {{ category.name }}
          </button>
        </div>
      </div>
      
      <!-- Gallery Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
        <div 
          v-for="(image, index) in filteredImages" 
          :key="index"
          class="relative overflow-hidden rounded-lg shadow-md cursor-pointer group"
          @click="openLightbox(index)"
        >
          <img 
            :src="image.src" 
            :alt="image.alt" 
            class="w-full h-64 object-cover transition-transform duration-300 group-hover:scale-105"
          />
          <div class="absolute inset-0 bg-black bg-opacity-20 opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-end">
            <div class="p-4 w-full bg-gradient-to-t from-black to-transparent">
              <p class="text-white font-medium">{{ image.caption }}</p>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Lightbox -->
      <div v-if="lightboxOpen" class="fixed inset-0 z-50 bg-black bg-opacity-90 flex items-center justify-center">
        <button 
          @click="closeLightbox" 
          class="absolute top-4 right-4 text-white hover:text-gray-300 focus:outline-none"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
        
        <button 
          @click="prevImage" 
          class="absolute left-4 text-white hover:text-gray-300 focus:outline-none"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
          </svg>
        </button>
        
        <div class="max-w-4xl max-h-[80vh] mx-auto">
          <img 
            :src="currentImage.src" 
            :alt="currentImage.alt" 
            class="max-h-[80vh] max-w-full object-contain"
          />
          <div class="mt-4 text-center">
            <p class="text-white text-lg">{{ currentImage.caption }}</p>
          </div>
        </div>
        
        <button 
          @click="nextImage" 
          class="absolute right-4 text-white hover:text-gray-300 focus:outline-none"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
          </svg>
        </button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';
  
  const activeCategory = ref('all');
  const lightboxOpen = ref(false);
  const currentImageIndex = ref(0);
  
  const categories = [
    { id: 'all', name: 'All Photos' },
    { id: 'engagement', name: 'Engagement' },
    { id: 'prewedding', name: 'Pre-Wedding' },
    { id: 'couple', name: 'Couple' }
  ];
  
  // Sample gallery images - in a real app, these would come from a database or API
  const images = [
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Engagement Photo 1',
      caption: 'Our engagement day',
      category: 'engagement'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Engagement Photo 2',
      caption: 'The proposal',
      category: 'engagement'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Pre-Wedding Photo 1',
      caption: 'Pre-wedding photoshoot',
      category: 'prewedding'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Pre-Wedding Photo 2',
      caption: 'Sunset session',
      category: 'prewedding'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Couple Photo 1',
      caption: 'Our first date',
      category: 'couple'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Couple Photo 2',
      caption: 'Anniversary celebration',
      category: 'couple'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Couple Photo 3',
      caption: 'Weekend getaway',
      category: 'couple'
    },
    {
      src: '/placeholder.svg?height=400&width=600',
      alt: 'Engagement Photo 3',
      caption: 'Ring close-up',
      category: 'engagement'
    }
  ];
  
  const filteredImages = computed(() => {
    if (activeCategory.value === 'all') {
      return images;
    }
    return images.filter(image => image.category === activeCategory.value);
  });
  
  const currentImage = computed(() => {
    return filteredImages.value[currentImageIndex.value];
  });
  
  const openLightbox = (index) => {
    currentImageIndex.value = index;
    lightboxOpen.value = true;
    document.body.style.overflow = 'hidden';
  };
  
  const closeLightbox = () => {
    lightboxOpen.value = false;
    document.body.style.overflow = '';
  };
  
  const nextImage = () => {
    currentImageIndex.value = (currentImageIndex.value + 1) % filteredImages.value.length;
  };
  
  const prevImage = () => {
    currentImageIndex.value = (currentImageIndex.value - 1 + filteredImages.value.length) % filteredImages.value.length;
  };
  </script>  