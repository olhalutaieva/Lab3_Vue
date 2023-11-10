<!-- src/components/TaskList.vue -->
<template>
    <div>
      <ul>
        <li v-for="task in tasks" :key="task.id">
          <span :class="{ 'completed': task.completed }">{{ task.text }}</span>
          <button @click="completeTask(task)">Complete</button>
          <button @click="deleteTask(task)">Delete</button>
        </li>
      </ul>
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Add new task" />
      <button @click="addTask">Add Task</button>
    </div>
  </template>
  
  <script>
  import { Task } from '@/types.ts';
  import { TaskRepository } from '@/repositories/TaskRepository';
  
  export default {
    data() {
      return {
        newTask: '',
        tasks: [],
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          const createdTask = this.repository.createTask(this.newTask);
          this.tasks.push(createdTask);
          this.newTask = '';
        }
      },
      completeTask(task: Task) {
        task.completed = true;
        this.repository.updateTask(task);
      },
      deleteTask(task: Task) {
        this.repository.deleteTask(task);
        const taskIndex = this.tasks.findIndex((t) => t.id === task.id);
        if (taskIndex !== -1) {
          this.tasks.splice(taskIndex, 1);
        }
      },
    },
    created() {
      this.repository = new TaskRepository(); // Ініціалізуємо репозиторій
      this.tasks = this.repository.getTasks();
    },
  };
  </script>
  
  <style scoped>
  .completed {
    text-decoration: line-through;
  }
  </style>
  