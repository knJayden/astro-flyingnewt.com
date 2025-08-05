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
              'px-4 py-2 text-sm md:text-md font-medium',
              activeCategory === category.id 
                ? 'bg-green text-zinc-100' 
                : 'bg-zinc-100 text-zinc-900 hover:bg-zinc-200 border-r-1 border border-zinc-200',
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
    { id: 'engagement', name: 'Engagement Shoot' },
    { id: 'proposal', name: 'The Proposal' },
    { id: 'couple', name: 'Just Us' }
  ];
  
  // Sample gallery images - in a real app, these would come from a database or API
  const images = [
    {
      src: '/src/assets/engagement-6.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Ring',
      category: 'engagement'
    },
 {
      src: '/src/assets/engagement-3.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Funny',
      category: 'engagement'
    },
   {
      src: '/src/assets/proposal-4.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Biltmore Estate',
      category: 'proposal'
    },
{
      src: '/src/assets/proposal-1.png',
      alt: 'Jayden and Makenzie',
      caption: 'On Bended Knee',
      category: 'proposal'
    },
      {
      src: '/src/assets/proposal-2.png',
      alt: 'Jayden and Makenzie',
      caption: 'Slipping the Ring on',
      category: 'proposal'
    },
      {
      src: '/src/assets/proposal-3.png',
      alt: 'Jayden and Makenzie',
      caption: 'She Said Yes!',
      category: 'proposal'
    },
    {
      src: '/src/assets/couple-1.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Road Trippin',
      category: 'couple'
    },
 {
      src: '/src/assets/proposal-5.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Biltmore Botanical Gardens',
      category: 'proposal'
    },

     {
      src: '/src/assets/engagement-1.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Holding Hands',
      category: 'engagement'
    },
{
      src: '/src/assets/couple-13.png',
      alt: 'Jayden and Makenzie',
      caption: 'Flying High',
      category: 'couple'
    },

      {
      src: '/src/assets/proposal-6.png',
      alt: 'Jayden and Makenzie',
      caption: 'Blue Ridge Mountain Hike',
      category: 'proposal'
    },
 {
      src: '/src/assets/engagement-2.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Piggy Back',
      category: 'engagement'
    },
    {
      src: '/src/assets/proposal-12.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Dinner',
      category: 'proposal'
    },
  {
      src: '/src/assets/couple-12.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Disney Adventures',
      category: 'couple'
    },
    {
      src: '/src/assets/engagement-4.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Even more Funny',
      category: 'engagement'
    },
  {
      src: '/src/assets/proposal-11.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'River Walk',
      category: 'proposal'
    },
{
      src: '/src/assets/couple-11.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Europe Trip',
      category: 'couple'
    },
      {
      src: '/src/assets/proposal-7.png',
      alt: 'Jayden and Makenzie',
      caption: 'From up above',
      category: 'proposal'
    },
 {
      src: '/src/assets/engagement-5.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
{
      src: '/src/assets/proposal-10.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'House in the Hills',
      category: 'proposal'
    },
 {
      src: '/src/assets/engagement-8.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
{
      src: '/src/assets/couple-10.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Rooftop Fun',
      category: 'couple'
    },
 {
      src: '/src/assets/engagement-9.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
 {
      src: '/src/assets/proposal-9.png',
      alt: 'Jayden and Makenzie',
      caption: 'The aforementioned Asparagus Tree',
      category: 'proposal'
    },
      {
      src: '/src/assets/couple-9.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Freemont St.',
      category: 'couple'
    },
  {
      src: '/src/assets/couple-7.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'On the Water',
      category: 'couple'
    },
 {
      src: '/src/assets/engagement-10.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
    {
      src: '/src/assets/couple-14.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'In the Snow',
      category: 'couple'
    },
  {
      src: '/src/assets/engagement-12.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
{
      src: '/src/assets/couple-8.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Grinch ICE',
      category: 'couple'
    },
{
      src: '/src/assets/proposal-8.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Biltmore Estate',
      category: 'proposal'
    },
      
   {
      src: '/src/assets/couple-3.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Botanical Gardens',
      category: 'couple'
    },
{
      src: '/src/assets/couple-4.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Beach',
      category: 'couple'
    },

    {
      src: '/src/assets/engagement-11.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
{
      src: '/src/assets/couple-5.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Halloween',
      category: 'couple'
    },
{
      src: '/src/assets/engagement-13.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Engagement Photoshoot',
      category: 'engagement'
    },
{
      src: '/src/assets/couple-6.jpg',
      alt: 'Jayden and Makenzie',
      caption: 'Hiking',
      category: 'couple'
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