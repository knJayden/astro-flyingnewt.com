<template>
    <div class="countdown-container bg-zinc-800 border-zinc-800 border-b border-t border-solid pt-5 pb-5">

      
          
      <div class="grid grid-cols-4 md:grid-cols-4 gap-4 max-w-2xl mx-auto">
              
        <div class="text-center">
          <div class="text-3xl md:text-5xl font-bold text-green">{{ days }}</div>
          <div class="text-zinc-400 text-base">Days</div>
         
        </div>
        <div class="text-center">
          <div class="text-3xl md:text-5xl font-bold text-green">{{ hours }}</div>
           <div class="text-zinc-400 text-base">Hours</div>
        </div>
        <div class="text-center">
          <div class="text-3xl md:text-5xl font-bold text-green">{{ minutes }}</div>
           <div class="text-zinc-400 text-base">Minutes</div>
        </div>
        <div class="text-center">
          <div class="text-3xl md:text-5xl font-bold text-green">{{ seconds }}</div>
           <div class="text-zinc-400 text-base">Seconds</div>
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
  