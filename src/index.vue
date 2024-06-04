<template>
  <div class="h-screen bg-blue-900 flex justify-center items-center">
    <div class="flex flex-col bg-white rounded-[24px] w-full md:w-[85%] p-4 md:p-[40px]">
      <p class="text-4xl md:text-5xl font-bold flex items-center justify-center pb-4">To Do List</p>
      <div class="flex flex-col md:flex-row md:space-x-4 pb-4 md:pb-8">
        <input v-model="newTask" type="text" placeholder="Enter to do"
          class="border w-full rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500 transition">
        <button @click="addItem"
          class="bg-blue-600 p-2 rounded-lg w-full md:w-[130px] h-[40px] text-white hover:shadow-md hover:scale-105 hover:bg-blue-500 transition-transform mt-4 md:mt-0">Add</button>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 font-bold gap-4 ">
        <div class="ml-[auto] md:ml-[20%]">No</div>
        <div class="">Task Name</div>
        <div class="ml-[auto] md:ml-[28px]">Status</div>
      </div>
      <hr class="my-[20px]">
      <div v-for="item in todoList" :key="item.id"
        class="bg-white drop-shadow-xl p-5 rounded-[14px] mb-[15px] grid grid-cols-4 gap-4">
        <div class="ml-11">{{ item.id }}</div>
        <div>{{ item.name }}</div>
        <div :class="{ 'bg-green-500': item.status === 'Successfully', 'bg-[#F5B041]': item.status !== 'Successfully' }"
          class="bg-[#F5B041] w-full md:w-[60%] h-[70%] mt-[3px] text-white rounded-full text-center">{{ item.status
          }}</div>
        <div class="flex justify-end space-x-1">
          <button @click="toggleStatus(item)"
            class="bg-green-500 p-[5px] rounded-lg w-[100px] h-[40px] text-white hover:shadow-md hover:scale-105 hover:bg-[#58D68D] transition-transform">Success</button>
          <button @click="editItem(item)" :disabled="item.status === 'Successfully'"
            class="bg-[#E67E22] p-[5px] rounded-[10px] w-[80px] h-[40px] text-white hover:shadow-xl hover:ease-in-out hover:scale-110 hover:bg-[#F8C471] transition-transform">Edit</button>
          <button @click="deleteItem"
            class="bg-[#E74C3C] p-[5px] rounded-[10px] w-[80px] h-[40px] text-white hover:shadow-xl hover:ease-in-out hover:scale-110 hover:bg-[#EC7063] transition-transform">Delete</button>
        </div>
      </div>

      <!-- Popup Modal -->
      <div v-if="isEditingModalVisible"
        class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-gray-800 bg-opacity-75">
        <div class="bg-white p-8 rounded-lg">
          <p class="text-lg font-bold mb-4">Edit Task Name</p>
          <input v-model="editedTaskName" type="text" class="border p-2 mb-4 w-full">
          <div class="flex justify-end">
            <button @click="saveEdit"
              class="bg-[#28B463] text-white px-4 py-2 rounded-[10px] mr-1 hover:shadow-xl hover:ease-in-out hover:scale-110 hover:bg-[#2ECC71] transition-transform">Save</button>
            <button @click="cancelEdit"
              class="bg-[#E74C3C] text-white px-4 py-2 rounded-[10px] hover:shadow-xl hover:ease-in-out hover:scale-110 hover:bg-[#EC7063] transition-transform">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import axios from 'axios';


export default {
  data() {
    return {
      newTask: '',
      todoList: [],
      editingItem: null,
      isEditingModalVisible: false,
      editedTaskName: '',
    };
  },
  methods: {
    
    addItem() {
      if (this.newTask.trim()) {
        const newItem = {
          id: this.todoList.length + 1,
          name: this.newTask,
          status: 'In Progress',
        };
        this.todoList.push(newItem);
        this.newTask = '';
      }
    },
    toggleStatus(item) {
      const itemIndex = this.todoList.findIndex(todo => todo.id === item.id);
      if (itemIndex > -1) {
        this.todoList[itemIndex].status = 'Successfully';
      }
    },
    deleteItem(index) {
      this.todoList.splice(index, 1);
    },
    editItem(item) {
      this.editingItem = item.id;
      this.editedTaskName = item.name;
      this.isEditingModalVisible = true;
    },
    saveEdit() {
      if (this.editedTaskName.trim()) {
        const itemIndex = this.todoList.findIndex(item => item.id === this.editingItem);
        if (itemIndex > -1) {
          this.todoList[itemIndex].name = this.editedTaskName;
          this.editingItem = null;
          this.isEditingModalVisible = false;
        }
      }
    },
    cancelEdit() {
      this.isEditingModalVisible = false;
    },
    
  },
};
</script>