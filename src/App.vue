<template>
  <div class="container">
    <h2>투두리스트</h2>
    <input v-model="newTask" placeholder="할 일을 입력하세요" @keyup.enter="addTask" />
    <button @click="addTask">추가</button>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <span :class="{ done: task.done }" @click="toggleTask(index)">
          <span v-if="task.done" class="check">&#x2714;</span>
          <span v-else class="unchecked">&#x25CB;</span>
          <span v-if="task.isEditing">
            <input v-model="task.text" @blur="stopEditing(index)" @keyup.enter="stopEditing(index)" />
            </span>
            <span v-else>
              {{ task.text }}
            </span>
        </span>
        <button @click="removeTask(index)" class="delete-btn">🗑️</button>
        <button @click="editTask(index)" class="edit-btn">✏️</button>

      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: []
    };
  },
  methods: {
    addTask() {
      const text = this.newTask.trim();
      if (text) {
        this.tasks.push({ text, done: false, isEditing: false });
        this.newTask = '';
        this.saveTasks();
      }
    },
    editTask(index) {
      this.tasks[index].isEditing = true;
    },
    stopEditing(index) {
      this.tasks[index].isEditing = false;
      this.saveTasks();
    },
    toggleTask(index) {
      if (!this.tasks[index].isEditing)
      this.tasks[index].done = !this.tasks[index].done;
      this.saveTasks();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks(); 
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  },
    mounted() {
      const saved = localStorage.getItem('tasks');
      if (saved) {
        this.tasks = JSON.parse(saved);
      }
    }
};
</script>

<style scoped>
.done {
  text-decoration: line-through;
  color: gray;
  }
  .container {
  max-width: 500px;
  margin: 80px auto;
  padding: 20px;
  border: 2px solid #ddd;
  border-radius: 10px;
  background-color: #f9f9f9;
}
.delete-btn {
  background: none;
  border: none;
  color: red;
  font-size: 20px;
  cursor: pointer;
  margin-right: 10px;
}
.edit-btn {
  background: none;
  border: none;
  color: blue;
  font-size: 20px;
  cursor: pointer;
}
input {
  padding: 5px;
  font-size: 16px;
}
</style>