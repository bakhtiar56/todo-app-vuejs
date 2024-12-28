<template>
  <div id="app">
    <h1> To-Do List</h1>
    <input v-model="newTask" placeholder="Add a new task" @keyup.enter="addTask" />
    <button id="addtask" @click="addTask">Add Task</button>
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" v-model="task.completed" />

        <span v-if="!task.isEditing" :class="{ 'completed-task': task.completed }">{{ task.text }}</span>
        <input v-else v-model="task.text" @keyup.enter="saveTask(task,index)" @blur="saveTask(task)" />

        <button v-if="!task.isEditing" @click="editTask(task)">Edit</button>
        <button @click="removeTask(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'App',
    data() {
    return {
      newTask: '',
      tasks: []
    };
  },
  mounted() {
  this.tasks = JSON.parse(localStorage.getItem('tasks')) || [];
},
 methods: {
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ text: this.newTask, completed: false, isEditing: false });
        this.newTask = '';
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    editTask(task) {
      task.isEditing = true;
    },
    saveTask(task, index) {
      task.isEditing = false;
      let newTask={text: task.text, completed: task.completed, isEditing: task.isEditing }
      this.tasks[index]=newTask
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
,

};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
html, body, div, span,
h1, p, ul, li {
  padding: 0;
  border: 0;
  font: inherit;
  vertical-align: baseline;
}

body {
  background: #f1f1f1;
  font-size: 16px;
  line-height: 22px;
  color: #404040;
  font-family: 'Lucida Grande', Verdana, sans-serif;
}

ol, ul {
  list-style: none;
}

.completed-task {
  text-decoration: line-through;
  color: grey;
}
input {
  margin-bottom: 10px;
  padding: 5px;
}
button {
 background: lightseagreen;
  color: #fff;
  padding: 3px 10px;
  margin: 0 0 0 3px;
  border: none;
  border-radius: 5px;
  font-size: 12px;
  line-height: 24px;
  cursor: pointer;
  vertical-align: bottom;
}
#addtask{
  margin-bottom:10px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 5px 0;
}
</style>  