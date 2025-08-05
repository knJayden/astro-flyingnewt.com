<template>
    <form @submit.prevent="submitForm" class="space-y-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div>
          <label for="firstName" class="block text-xl font-medium text-zinc-900 mb-1">First Name</label>
          <input 
            type="text" 
            id="firstName" 
            v-model="form.firstName" 
            required
            class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
          />
        </div>
        <div>
          <label for="lastName" class="block text-xl font-medium text-zinc-900 mb-1">Last Name</label>
          <input 
            type="text" 
            id="lastName" 
            v-model="form.lastName" 
            required
            class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
          />
        </div>
      </div>
      
      <div>
        <label for="email" class="block text-xl font-medium text-zinc-900 mb-1">Email</label>
        <input 
          type="email" 
          id="email" 
          v-model="form.email" 
          required
          class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
        />
      </div>
      
      <div>
        <label class="block text-xl font-medium text-gray-700 mb-1">Will you be attending?</label>
        <div class="flex space-x-4">
          <label class="inline-flex items-center">
            <input 
              type="radio" 
              v-model="form.attending" 
              :value="true" 
              class="text-rose-600 focus:ring-rose-500"
            />
            <span class="ml-2 text-zinc-900 text-md">Yes, I'll be there!</span>
          </label>
          <label class="inline-flex items-center">
            <input 
              type="radio" 
              v-model="form.attending" 
              :value="false" 
              class="text-rose-600 focus:ring-rose-500"
            />
            <span class="ml-2 text-zinc-900">Sorry, I can't make it</span>
          </label>
        </div>
      </div>
      
      <div v-if="form.attending">
        <label for="guests" class="block text-lg font-medium text-gray-700 mb-1">Number of Guests (including yourself)</label>
        <select 
          id="guests" 
          v-model="form.guestCount" 
          class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
        >
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
        </select>
      </div>
      
      <div v-if="form.attending">
        <label for="dietaryRestrictions" class="block text-lg font-medium text-gray-700 mb-1">Dietary Restrictions</label>
        <textarea 
          id="dietaryRestrictions" 
          v-model="form.dietaryRestrictions" 
          rows="3"
          placeholder="Please let us know if you have any dietary restrictions or allergies."
          class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
        ></textarea>
      </div>
      
      <div>
        <label for="message" class="block text-lg font-medium text-gray-700 mb-1">Message (Optional)</label>
        <textarea 
          id="message" 
          v-model="form.message" 
          rows="3"
          placeholder="Share a message or note for the couple."
          class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-rose-500"
        ></textarea>
      </div>
      
      <div class="flex justify-center">
        <button 
          type="submit" 
          class="px-6 py-3 bg-green text-zinc-100 font-semibold rounded-md transition"
          :disabled="submitting"
        >
          {{ submitting ? 'Submitting...' : 'Submit RSVP' }}
        </button>
      </div>
      
      <div v-if="submitted" class="text-center p-4 bg-green-100 text-zinc-900 rounded-md">
        Thank you for your RSVP! We've received your response.
      </div>
    </form>
  </template>
  
  <script setup>
  import { ref, reactive } from 'vue';
  
  const form = reactive({
    firstName: '',
    lastName: '',
    email: '',
    attending: null,
    guestCount: '1',
    dietaryRestrictions: '',
    message: ''
  });
  
  const submitting = ref(false);
  const submitted = ref(false);
  
  const submitForm = async () => {
    submitting.value = true;
    
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // In a real application, you would send the form data to your backend
    console.log('Form submitted:', form);
    
    submitting.value = false;
    submitted.value = true;
    
    // Reset form after submission
    setTimeout(() => {
      form.firstName = '';
      form.lastName = '';
      form.email = '';
      form.attending = null;
      form.guestCount = '1';
      form.dietaryRestrictions = '';
      form.message = '';
      submitted.value = false;
    }, 5000);
  };
  </script>
  