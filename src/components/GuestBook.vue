<template>
    <div class="guest-book">
      <!-- Add Comment Form -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-8">
        <h3 class="text-xl font-bold text-gray-800 mb-4">Leave a Message</h3>
        <form @submit.prevent="addComment" class="space-y-4">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="name" class="block text-sm font-medium text-gray-700 mb-1">Your Name</label>
              <input 
                type="text" 
                id="name" 
                v-model="newComment.name" 
                required
                class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
              />
            </div>
            <div>
              <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email (not displayed)</label>
              <input 
                type="email" 
                id="email" 
                v-model="newComment.email" 
                required
                class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
              />
            </div>
          </div>
          <div>
            <label for="message" class="block text-sm font-medium text-gray-700 mb-1">Your Message</label>
            <textarea 
              id="message" 
              v-model="newComment.message" 
              rows="4"
              required
              class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
            ></textarea>
          </div>
          <div class="flex justify-end">
            <button 
              type="submit" 
              class="px-4 py-2 bg-rose-600 text-white font-semibold rounded-md hover:bg-rose-700 focus:outline-none focus:ring-2 focus:ring-rose-500 focus:ring-offset-2 transition"
              :disabled="submitting"
            >
              {{ submitting ? 'Posting...' : 'Post Message' }}
            </button>
          </div>
        </form>
      </div>
      
      <!-- Comments List -->
      <div class="space-y-6">
        <div v-if="comments.length === 0" class="text-center text-gray-500 py-8">
          Be the first to leave a message!
        </div>
        
        <div v-for="(comment, index) in comments" :key="index" class="bg-white rounded-lg shadow-md p-6">
          <div class="flex justify-between items-start mb-4">
            <div>
              <h4 class="text-lg font-bold text-gray-800">{{ comment.name }}</h4>
              <p class="text-sm text-gray-500">{{ formatDate(comment.date) }}</p>
            </div>
          </div>
          <p class="text-gray-700">{{ comment.message }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, reactive } from 'vue';
  
  const submitting = ref(false);
  
  const newComment = reactive({
    name: '',
    email: '',
    message: ''
  });
  
  // Sample comments data - in a real app, this would come from a database
  const comments = ref([
    {
      name: 'Emily Johnson',
      message: 'Congratulations on your engagement! Can\'t wait to celebrate with you both!',
      date: new Date('2024-01-15T14:32:00')
    },
    {
      name: 'Michael Thompson',
      message: 'So excited for you two! Looking forward to the big day!',
      date: new Date('2024-01-10T09:15:00')
    },
    {
      name: 'Sarah Williams',
      message: 'You two are perfect for each other. Wishing you a lifetime of happiness!',
      date: new Date('2024-01-05T16:45:00')
    }
  ]);
  
  const addComment = async () => {
    submitting.value = true;
    
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // Add new comment to the list
    comments.value.unshift({
      name: newComment.name,
      message: newComment.message,
      date: new Date()
    });
    
    // Reset form
    newComment.name = '';
    newComment.email = '';
    newComment.message = '';
    
    submitting.value = false;
  };
  
  const formatDate = (date) => {
    return new Intl.DateTimeFormat('en-US', {
      year: 'numeric',
      month: 'long',
      day: 'numeric'
    }).format(date);
  };
  </script>
  