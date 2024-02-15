<template>
  <div>
    <ItemList :items="items" @edit-item="editItem" @delete-item="handleDeleteItem" />
    <!-- Conditionally render either the AddItemForm or EditItemForm -->
    <AddItemForm v-if="isAddingItem" @submit="addItem" />
    <EditItemForm v-if="isEditingItem" :item="selectedItem" @submit="updateItem" />
  </div>
</template>

<script>
import axios from 'axios';
import ItemList from './components/ItemList.vue';
import AddItemForm from './components/AddItemForm.vue';
import EditItemForm from './components/EditItemForm.vue';

export default {
  components: {
    ItemList,
    AddItemForm,
    EditItemForm
  },
  data() {
    return {
      items: [],
      isAddingItem: false,
      isEditingItem: false,
      selectedItem: null
    };
  },
  created() {
    this.fetchItems();
  },
  methods: {
    fetchItems() {
      axios.get('http://localhost:3000/items')
        .then(response => {
          this.items = response.data;
        })
        .catch(error => {
          console.error('Error fetching items:', error);
        });
    },
    editItem(item) {
      this.selectedItem = item;
      this.isEditingItem = true;
    },
    addItem(newItemData) {
      this.isAddingItem = false;
      this.fetchItems(); 
    },
    updateItem(updatedItemData) {
      this.isEditingItem = false;
      this.fetchItems(); 
    },
    deleteItem(itemId) {
      axios.delete(`http://localhost:3000/items/${itemId}`)
        .then(() => {
          this.items = this.items.filter(item => item._id !== itemId);
        })
        .catch(error => {
          console.error('Error deleting item:', error);
        });
    },
    showAddForm() {
      this.showForm = true;
      this.formTitle = 'Add Item';
      this.buttonLabel = 'Add';
      this.formData = {
        name: '',
        description: '',
        price: 0
      };
    },
    showItemForm(item) {
      this.showForm = true;
      this.formTitle = 'Edit Item';
      this.buttonLabel = 'Save';
      this.formData = { ...item };
    },
    async handleDelete(itemId) {
      try {
        await axios.delete(`http://localhost:3000/items/${itemId}`);
        // Remove the deleted item from the items list
        this.items = this.items.filter(item => item._id !== itemId);
      } catch (error) {
        console.error('Error deleting item:', error);
      }
    },
    handleSubmit(itemData) {
      if (itemData._id) {
        // Edit item
        axios.put(`http://localhost:3000/items/${itemData._id}`, itemData)
          .then(() => {
            // Update item in items array
            const index = this.items.findIndex(item => item._id === itemData._id);
            if (index !== -1) {
              this.items[index] = itemData;
            }
            this.closeForm();
          })
          .catch(error => {
            console.error('Error updating item:', error);
          });
      } else {
        // Add item
        axios.post('http://localhost:3000/items', itemData)
          .then(response => {
            this.items.push(response.data);
            this.closeForm();
          })
          .catch(error => {
            console.error('Error adding item:', error);
          });
      }
    },
    handleDeleteItem(itemId) {
      // Call delete endpoint to delete the item
      axios.delete(`http://localhost:3000/items/${itemId}`)
        .then(response => {
          console.log('Item deleted successfully');
          // Fetch updated items after deletion
          this.fetchItems();
        })
        .catch(error => {
          console.error('Error deleting item:', error);
        });
    },
    closeForm() {
      this.showForm = false;
      this.selectedItem = null;
    },
    toggleItemForm() {
      this.showForm = !this.showForm;
    }
  }
};
</script>

<style>
/* Add global CSS styles if needed */
</style>
