<template>
    <form @submit.prevent="handleSubmit">
      <input type="text" v-model="name" placeholder="Name" required>
      <input type="text" v-model="description" placeholder="Description" required>
      <input type="number" v-model="price" placeholder="Price" required>
      <button type="submit">{{ buttonText }}</button>
    </form>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: ['buttonText'],
    data() {
      return {
        name: '',
        description: '',
        price: null
      };
    },
    methods: {
      handleSubmit() {
        const newItem = {
          name: this.name,
          description: this.description,
          price: this.price
        };
        axios.post('http://localhost:3000/items', newItem)
          .then(response => {
            // Emit an event to notify the parent component that an item has been added
            this.$emit('itemAdded', response.data);
            // Clear input fields
            this.name = '';
            this.description = '';
            this.price = null;
          })
          .catch(error => {
            console.error('Error adding item:', error.response.data);
          });
      }
    }
  };
  </script>
  