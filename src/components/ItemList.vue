<template>
  <div>
    <h2>Items List</h2>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Description</th>
          <th>Price</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item._id">
          <td>{{ item.name }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.price }}</td>
          <td>
            <button @click="editItem(item)">Edit</button>
            <button @click="deleteItem(item._id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
    
    <!-- Conditional rendering of AddItemForm or EditItemForm -->
    <div v-if="showAddForm">
      <h2>Add New Item</h2>
      <AddItemForm @submit="addItem" />
    </div>

    <div v-else-if="showEditForm">
      <h2>Edit Item</h2>
      <EditItemForm :item="selectedItem" @submit="updateItem" />
    </div>

    <!-- Button to toggle between AddItemForm and ItemList -->
    <button @click="toggleForm">{{ showAddForm ? 'Cancel' : 'Add Item' }}</button>
  </div>
</template>

<script>
import AddItemForm from './AddItemForm.vue';
import EditItemForm from './EditItemForm.vue';

export default {
  components: {
    AddItemForm,
    EditItemForm
  },
  props: ['items'],
  data() {
    return {
      showAddForm: false,
      showEditForm: false,
      selectedItem: null
    };
  },
  methods: {
    editItem(item) {
      this.selectedItem = item;
      this.showEditForm = true;
    },
    deleteItem(itemId) {
      this.$emit('delete-item', itemId);
    },
    addItem(itemData) {
      // Implement add logic
      this.showAddForm = false;
    },
    updateItem(itemData) {
      // Implement update logic
      this.showEditForm = false;
    },
    toggleForm() {
      this.showAddForm = !this.showAddForm;
      this.showEditForm = false;
      this.selectedItem = null;
    }
  }
};
</script>

<style scoped>
/* Add scoped CSS styles if needed */
</style>
