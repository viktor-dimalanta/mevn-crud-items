<template>
  <div>
    <h2>Add New Item</h2>
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
      <button type="submit">Add Item</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      formData: {
        name: '',
        description: '',
        price: 0
      }
    };
  },
  methods: {
    handleSubmit() {
        const newItem = {
          name: this.formData.name,
          description: this.formData.description,
          price: this.formData.price
        };
        axios.post('http://localhost:3000/items', newItem)
          .then(response => {
            // Emit an event to notify the parent component that an item has been added
            this.$emit('itemAdded', response.data);
            // Clear input fields
            this.clearFormData();
          })
          .catch(error => {
            console.error('Error adding item:', error.response.data);
          });
      },
    clearFormData() {
      // Reset form data to initial values
      this.formData = {
        name: '',
        description: '',
        price: 0
      };
    }
  }
};
</script>
