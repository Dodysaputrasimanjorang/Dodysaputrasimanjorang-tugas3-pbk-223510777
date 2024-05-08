<template>
  <div id="app">
    <h1>ToDo List App</h1>
    <div class="input-container">
      <input v-model="newItem" @keyup.enter="addItem" type="text" placeholder="Add New Item">
      <button @click="addItem">Add</button>
    </div>
    <ul>
      <li v-for="(item, index) in items" :key="index" :class="{ completed: item.completed }">
        <input type="checkbox" v-model="item.completed">
        <span>{{ item.text }}</span>
        <div class="button-container">
          <button @click="editItem(index)">Edit</button>
          <button @click="removeItem(index)">Delete</button>
        </div>
      </li>
    </ul>
    <div v-if="editIndex !== null" class="edit-container">
      <input v-model="editText" @keyup.enter="updateItem" type="text" placeholder="Edit Item">
      <button @click="updateItem">Update</button>
      <button @click="cancelEdit">Cancel</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      newItem: '',
      items: [],
      editIndex: null,
      editText: ''
    };
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push({ text: this.newItem, completed: false });
        this.newItem = '';
      }
    },
    removeItem(index) {
      this.items.splice(index, 1);
    },
    editItem(index) {
      this.editIndex = index;
      this.editText = this.items[index].text;
    },
    updateItem() {
      if (this.editIndex !== null && this.editText.trim() !== '') {
        this.items[this.editIndex].text = this.editText;
        this.cancelEdit();
      }
    },
    cancelEdit() {
      this.editIndex = null;
      this.editText = '';
    }
  }
};
</script>

<style scoped>
#app {
  max-width: 500px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

.input-container {
  display: flex;
  margin-bottom: 10px;
}

input[type="text"] {
  flex: 1;
  padding: 8px;
  font-size: 16px;
}

button {
  padding: 8px 16px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  margin-bottom: 5px;
}

.completed {
  text-decoration: line-through;
  color: #888;
}

.button-container {
  margin-left: auto;
  display: flex;
}

.edit-container {
  margin-top: 10px;
  display: flex;
  align-items: center;
}

.edit-container input[type="text"] {
  flex: 1;
}

.edit-container button {
  margin-left: 5px;
}
</style>
