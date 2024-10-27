<!-- pages/tasks.vue -->
<template>
    <div>
      <h1>Task Manager</h1>
  
      <!-- Create Task Form -->
      <form @submit.prevent="addTask">
        <input v-model="newTask.title" placeholder="Task title" required />
        <button type="submit">Add Task</button>
      </form>
  
      <ul>
        <li v-for="(task, index) in tasks" :key="task.id">
          <!-- Editable title -->
          <span v-if="task.id !== editId">{{ task.title }}</span>
          <input
            v-else
            v-model="editTask.title"
            @keyup.enter="updateTask(task.id)"
            placeholder="Edit task title"
          />
  
          <!-- Edit and Delete buttons -->
          <button @click="startEditing(task)">Edit</button>
          <button @click="deleteTask(task.id)">Delete</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newTask: { title: '' }, // For adding new task
        tasks: [],              // Task list
        editTask: {},           // For editing an existing task
        editId: null            // ID of task currently being edited
      };
    },
    methods: {
      // Create a new task
      addTask() {
        if (this.newTask.title.trim()) {
          this.tasks.push({ id: Date.now(), title: this.newTask.title });
          this.newTask.title = ''; // Reset new task input
        }
      },
      // Start editing a task
      startEditing(task) {
        this.editId = task.id;
        this.editTask = { ...task }; // Copy task for editing
      },
      // Update task title
      updateTask(id) {
        const taskIndex = this.tasks.findIndex(t => t.id === id);
        if (taskIndex !== -1) {
          this.tasks[taskIndex].title = this.editTask.title;
        }
        this.editId = null; // Reset edit ID after updating
      },
      // Delete a task
      deleteTask(id) {
        this.tasks = this.tasks.filter(task => task.id !== id);
      }
    }
  };
  </script>
  
  <style scoped>
  /* Basic styling */
  h1 {
    font-size: 24px;
    margin-bottom: 16px;
  }
  form {
    display: flex;
    gap: 8px;
    margin-bottom: 16px;
  }
  button {
    margin-left: 8px;
  }
  </style>
  