<template>
  <div class="list-task">
    <div class="list-header">
      <div class="navbar d-flex justify-content-center align-items-center">
        <p class="navleft" style="margin-right: auto">Thufeil23</p>
        <img src="public/icon-green.svg" class="navicon">
        <p class="navright" style="margin-left: auto">/THVL</p>
      </div>

      <div class="list-border">
        <h1 class="list-h1">ToDo<span class="accent-color">'</span>s <span class="accent-color">App</span></h1>
      </div>
      <div class="list-borderless">
        <div v-if="tasks.length === 0" class="no-tasks" style="display: flex; justify-content: space-between;">
          <div>
            <p class="list-p1">Hey there,</p>
            <p class="list-p2">it looks like you're all caught up!</p>
            <p class="list-p2">No tasks to do just yet.</p>
            <p class="list-p3">But don't worry, it's time to add some excitement to your day with new tasks, unless you've already conquered them all! Way to go, you're doing an amazing job!</p>
          </div>
          <div class="task-counter" style="padding-left: 250px">{{ getTaskCount() }}</div>
        </div>
        <div v-else class="no-tasks" style="display: flex; justify-content: space-between;">
          <div>
            <p class="list-p1">Hey there,</p>
            <p class="list-p2">you've got something to do</p>
            <p class="list-p2"> Check your list!</p>
            <p class="list-p3">Let's get up and do those tasks, instead of endlessly scrolling through your phone! You've got this!</p>
          </div>
          <div class="task-counter" style="padding-left: 250px">{{ getTaskCount() }}</div>
        </div>
        <div class="action py-2">
          <a @click="toggleisCreating" v-if="isCreating" href="#" class="add-button">Add Task +</a>
          <div class="add-card" v-else>
            <div class="card">
              <div class="card-body d-flex flex-column">
                <p class="card-title">New Task</p>
                <div class="form-group">
                  <input v-model="titleValue" class="form-control" placeholder="Title" type="text">
                </div>
                <div class="form-group">
                  <textarea v-model="descriptionValue" class="form-control" placeholder="Description" rows="3"></textarea>
                </div>
              </div>
            </div>
            <div class="button-wrapper d-flex justify-content-end">
              <button class="btn btn-primary" @click="addTask">Save</button>
              <button class="btn btn-outline-secondary" @click="toggleisCreating">Cancel</button>
            </div>
          </div>
          <div class="line"></div>
        </div>
        <div v-for="(item, index) of tasks" class="item-task" :key="item.id">
          <div :class="{ 'task--done': item.isDone }">
            <div class="id-task">
              {{ item.id }}
            </div>
            <div class="task-input">
                <div class="title-task" :class="{ 'task--done': item.isDone }">
                  {{ item.title }}
                </div>
                <div class="description-task text-muted" :class="{ 'task--done': item.isDone }">
                  {{ item.description }}
                </div>
            </div>
            <div class="task-btn">
              <input 
                type="checkbox"
                name="status"
                id="task"
                :checked="item.isDone"
                @change="toggleisDone(index)"
              />
              <button @click="deleteTask(item.id)" class="btn btn-danger">Delete</button>
              <button v-if="item.isDone" @click="deleteAllDoneTasks" class="btn btn-danger">Clear Completed Tasks</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style>

  :root {
    background: linear-gradient(-270deg,#EFEFFC, #F0F0FC, #C7C6D6);
    margin: 0;

  }
  .accent-color {
    color: #00C58E;
  }
  .list-task {
    font-family: 'Century Gothic', sans-serif;
  }
  .list-header {
    border-radius: 18px;
    padding-right: 12px;
    padding-left: 12px;
    padding-bottom: 20px;
  }
  .list-h1 {
    font-family: 'Century Gothic', sans-serif;
    color: #18181B;
    font-weight: bolder;
    font-style: italic;
    font-size: 44px;
    margin: 0;
    margin-bottom: 16px;
    margin-top: 16px;
  }
  .list-border {
    margin-right: 48px;
    margin-left: 48px;
    margin-top: 48px;
    margin-bottom: 8px;
    padding: 22px;
    padding-top: 2px;
    padding-bottom: 2px;
    border: 1px solid #3B3A41; 
    border-radius: 20px;
  }
  .list-borderless {
    margin-right: 46px;
    margin-left: 46px;
    margin-top: 12px;
    margin-bottom: 8px;
    padding: 28px;
    padding-top: 12px;
    padding-bottom: 2px;
  }
  .list-p1 {
    font-family: 'Century Gothic', sans-serif;
    color: #3B3A41;
    font-size: 26px;
    font-weight: bold;
    margin: 0;
    margin-bottom: 8px;
  }
  .list-p2 {
    font-family: 'Century Gothic', sans-serif;
    color: #3B3A41;
    font-size: 22px;
    font-weight: normal;
    margin: 0;
  }
  .list-p3 {
    font-family: 'Gotham Light', sans-serif;
    color: #6A6A6A;
    font-size: 22px;
    letter-spacing: 1px;
    margin-right: 12px;
  }
  .no-tasks {
    font-family: 'Gotham Light', sans-serif;
    display: flex;
    justify-content: space-between;
  }
  .task-counter {
    font-family: 'Century Gothic', sans-serif;
    font-weight: normal;
    color: #18181B;
    font-size: 312px;
    margin: 0;
    margin-right: 20px;
    margin-left: auto;
  }
  .navbar {
    display: flex;
    align-items: center;
    justify-content: center;
    border-bottom: 1px solid #1E1E1E;
    margin-bottom: 24px;
    font-weight: bold;
    color: #3B3A41;
    font-size: 18px;
  }
  .navright {
    font-family: 'Gotham black', sans-serif;
    letter-spacing: 2px;
    margin-right: 22px;
  }
  .navleft {
    font-family: 'Century Gothic', sans-serif;
    margin-left: 22px;
  }
  .navicon {
    width: 40px;
    height: 30px;
  }
  .item-task {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row; 
    justify-content: center;
    align-items: flex-start;
    align-content: start;
    border-bottom: 2px solid #18181B;
    padding-top: 20px;
    padding-bottom: 10px;
  }
  .id-task {
    font-weight: bold;
    font-size: 64px;
    flex: 0 0 auto;
    margin: 2;
  }
  .task-input {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: auto;
    align-content: start;
  }
  .task-btn {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row-reverse;
    justify-content: flex-end;
    align-items: flex-end;
    align-content: start;
  }
  .task--done {
    text-decoration: line-through;
    text-decoration-color: #00C58E;
    text-decoration-thickness: 2.5px; 
  }
  .title-task {
    font-weight: bold;
    color: #1E1E1E;
  }
  .description-task {
    font-size: 14px;
    color: #6A6A6A;
  }

  .btn {
    font-size: 14px;
    padding: 6px 12px;
    border-radius: 4px;
  }

  .btn-danger {
    background-color: #dc3545;
    border-color: #dc3545;
    color: #EFEFFC;
  }

  .btn-primary {
    background-color: #00C58E;
    border-color: #00C58E;
    color: #EFEFFC;
    margin: 4px;
  }

  .btn-outline-secondary {
    background-color: transparent;
    border-color: #6c757d;
    color: #6c757d;
    margin: 4px;
  }

  .checkbox {
    color: #00C58E;
    cursor: pointer;
    text-decoration: none;
  }

  .add-button {
    font-size: 18px;
    font-weight: bold;
    color: #00C58E;
    cursor: pointer;
    text-decoration: none;
    padding-bottom: 20px;
  }
  .line {
    margin-top: 10px;
    border-left: 0;
    border-right: 0;
    border-bottom: 0;
    border: 10px solid;
    border-image-slice: 1;
    border-width: 1px;
    border-image-source: linear-gradient(to right, #3B3A41 0%, #18181B 50%, #3B3A41 100%);
  }

  .add-card {
    margin-top: 10px;
  }

  .card {
    font-family: 'Century Gothic', sans-serif;
  }
  .card-title {
    color: #3B3A41;
    font-size: 18px;
    font-weight: bold;
    padding-bottom: 10px;
    margin: 0;
  }
  .form-group {
    margin-bottom: 8px;
  }
  .form-control {
    padding: 6px 12px;
    font-size: 14px;
    border-radius: 4px;
    border: 1px solid #F0F0FC;
    width: 512px;
  }

  .form-control:focus {
    outline: none;
    box-shadow: none;
    border-color: #00C58E;
  }

  .small {
    font-size: 12px;
  }

  .text-muted {
    color: #6A6A6A;
  }

</style>
<script>
export default {
  name: 'ToDoApp',
  data() {
    return {
      tasks: [],
      isCreating: true,
      titleValue: '',
      descriptionValue: '',


    }
  },
  methods: {
    toggleisCreating() {
      this.isCreating = !this.isCreating
    },
    getTaskCount() {
      return this.tasks.length
    },
    addTask() {
      this.tasks.push({
        id: this.tasks.length + 1,
        title: this.titleValue,
        description: this.descriptionValue,
        isDone: false,
      })
      console.log(this.tasks)
      this.titleValue = ''
      this.descriptionValue = ''
      this.toggleisCreating()
    },
    toggleisDone(index) {
      this.tasks[index].isDone = !this.tasks[index].isDone
    },
    deleteAllDoneTasks() {
      this.tasks = this.tasks.filter(task => task.isDone === false)
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    },
  }
}
</script>