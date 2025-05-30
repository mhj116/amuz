<template>
  <div :class="['container']">
    <h2>투두리스트</h2>
    <p>전체: {{ totalTasks }} / 완료: {{ completedTasks }} / 남은: {{ remainingTasks }}</p>

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
      tasks: [],
    };
  },
  computed: {
  totalTasks() {
    return this.tasks.length;
  },
  completedTasks() {
    return this.tasks.filter(task => task.done).length;
  },
  remainingTasks() {
    return this.tasks.filter(task => !task.done).length;
  }
},  
  methods: {
    toggleMode() {
    this.isDark = !this.isDark;
  },
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
.container {
  max-width: 500px;
  margin: 80px auto;
  padding: 30px;
  border: 2px solid #ddd;
  border-radius: 15px;
  background-color: #ffffff;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

input {
  padding: 10px;
  font-size: 16px;
  width: 70%;
  border: 1px solid #ccc;
  border-radius: 8px;
  margin-right: 10px;
}

button {
  padding: 8px 12px;
  font-size: 14px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  transition: 0.3s ease;
}

.delete-btn {
  color: red;
  background-color: #ffecec;
  margin-left: 5px;
}

.delete-btn:hover {
  background-color: #ffcccc;
}

.edit-btn {
  color: #0056d2;
  background-color: #e8f0ff;
  margin-left: 5px;
}

.edit-btn:hover {
  background-color: #d0e0ff;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f7f7f7;
  padding: 12px 15px;
  margin: 10px 0;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.03);
}
</style>