<template>
    <div class="countdown-container">
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-2xl mx-auto">
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl md:text-4xl font-bold text-rose-600">{{ days }}</div>
          <div class="text-gray-600">Days</div>
        </div>
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl md:text-4xl font-bold text-rose-600">{{ hours }}</div>
          <div class="text-gray-600">Hours</div>
        </div>
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl md:text-4xl font-bold text-rose-600">{{ minutes }}</div>
          <div class="text-gray-600">Minutes</div>
        </div>
        <div class="bg-white rounded-lg shadow-md p-4 text-center">
          <div class="text-3xl md:text-4xl font-bold text-rose-600">{{ seconds }}</div>
          <div class="text-gray-600">Seconds</div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue';
  
  const props = defineProps({
    weddingDate: {
      type: String,
      required: true
    }
  });
  
  const days = ref(0);
  const hours = ref(0);
  const minutes = ref(0);
  const seconds = ref(0);
  let intervalId = null;
  
  const calculateTimeLeft = () => {
    const weddingTime = new Date(props.weddingDate).getTime();
    const now = new Date().getTime();
    const difference = weddingTime - now;
    
    if (difference > 0) {
      days.value = Math.floor(difference / (1000 * 60 * 60 * 24));
      hours.value = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      minutes.value = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
      seconds.value = Math.floor((difference % (1000 * 60)) / 1000);
    }
  };
  
  onMounted(() => {
    calculateTimeLeft();
    intervalId = setInterval(calculateTimeLeft, 1000);
  });
  
  onUnmounted(() => {
    if (intervalId) {
      clearInterval(intervalId);
    }
  });
  </script>
  