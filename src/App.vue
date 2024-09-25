<template>
  <div id="app">
    <h1>Todo List</h1>
    <input
      v-model="newTask"
      placeholder="Add a new task"
      @keyup.enter="addTask"
    />
    <button class="add-task" @click="addTask">Add task</button>

    <ul>
      <li
        v-for="(task, index) in tasks"
        :key="index"
        :class="{ completed: task.completed }"
      >
        {{ task.text }}
        <div class="task-buttons">
          <button @click="completeTask(index)">✔️</button>
          <button @click="removeTask(index)">❌</button>
          <button @click="editTask(index)" v-if="!task.completed">✍️</button>
        </div>
        <input
          v-if="task.editing && !task.completed"
          v-model="task.text"
          @blur="saveTask(index)"
          @keyup.enter="saveTask(index)"
        />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  mounted() {
    this.tasks = JSON.parse(localStorage.getItem("tasks")) || [];
  },
  beforeUpdate() {
    localStorage.setItem("tasks", JSON.stringify(this.tasks));
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = "";
      }
    },
    completeTask(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.tasks[index].editing = true;
    },
    saveTask(index) {
      this.tasks[index].editing = false;
    },
  },
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

h1 {
  font-weight: 700;
  margin: 0;
  font-family: "Josefin Sans", sans-serif;
}

button {
  font-family: "Josefin Sans", sans-serif;
  font-optical-sizing: auto;
  font-weight: 400;
  font-style: normal;
}

.add-task {
  width: 100px;
  height: 45px;
  font-size: 18px;
  border-radius: 4px;
  border-style: none;
  transition: 0.25s;
  background-color: rgb(237, 221, 255);
}

.add-task:hover {
  transition: 0.2s;
  background-color: rgb(248, 168, 255);
}

li {
  background-color: rgb(237, 221, 255);
  border-radius: 25px;
  padding: 15px;
  margin-bottom: 15px;
  width: 250px;
  box-shadow: 10px 0px 20px rgba(0, 0, 0, 0.2);
}

.task-buttons {
  margin-top: 10px;
}

.completed {
  text-decoration: line-through;
}

input {
  font-size: 20px;
  width: 280px;
  height: 40px;
  border-radius: 25px;
  border: 1px solid gainsboro;
  padding-inline: 5%;
  box-shadow: 0 2px 7px rgba(0, 0, 0, 0.2);
}
</style>
