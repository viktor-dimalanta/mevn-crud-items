<template>
    <div>
      <h2>Edit Item</h2>
      <form @submit.prevent="handleSubmit">
        <div>
          <label for="name">Name:</label>
          <input type="text" id="name" v-model="formData.name" />
        </div>
        <div>
          <label for="description">Description:</label>
          <textarea id="description" v-model="formData.description"></textarea>
        </div>
        <div>
          <label for="price">Price:</label>
          <input type="number" id="price" v-model.number="formData.price" />
        </div>
        <button type="submit">Update Item</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    props: ['item'],
    data() {
      return {
        formData: {
          name: '',
          description: '',
          price: 0
        }
      };
    },
    watch: {
      item: {
        handler(newValue) {
          this.formData = { ...newValue }; // Populate formData with item data
        },
        immediate: true
      }
    },
    methods: {
      handleSubmit() {
        axios.put(`http://localhost:3000/items/${this.item._id}`, this.formData)
          .then(response => {
            // Emit an event to notify the parent component that an item has been updated
            this.$emit('itemUpdated', response.data);
          })
          .catch(error => {
            console.error('Error updating item:', error.response.data);
          });
      }
    }
  };
  </script>
  