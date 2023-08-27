<template>
  <div id="app">
    <div class="container">
      <div
        class="text-white bg-secondary p-3 mb-3 text-center d-flex justify-content-between align-item-center"
      >
        <div class="">
          <h1 class="pt-2">My ToDo List</h1>
        </div>
        <div class="text-end">
          <div class="form-check">
            <label for="flexCheckChecked" class="form-check-label"
              >Hide Complete Tasks</label
            >
            <input
              type="checkbox"
              id="flexCheckChecked"
              class="form-check-input"
              v-model="hideComplete"
            />
          </div>
          <button class="btn btn-danger d-block mt-2">
            Delete Complete Tasks
          </button>
        </div>
      </div>
      <div class="row mb-2">
        <div class="col-6 fs-2 fw-bold">Tasks</div>
        <div class="col-6 fs-2 fw-bold">Done</div>
      </div>
      <div class="row" v-for="(task, index) in filterTask" :key="index">
        <div class="col-6" :class="task.done ? 'deleteTask' : ''">
          {{ task.action }}
        </div>
        <div class="col-6">
          <input type="checkbox" v-model="task.done" id="" />
        </div>
      </div>
      <hr />
      <div class="row">
        <h3>Create Task</h3>
        <div class="col-6">
          <input
          type="text"
          class="form-control"
          v-model="createTask"
        />
        <small class="text-danger" v-if="valiMessage">Please Fill Your task...</small>
        </div>
        <div class="col-6">
          <button class="btn btn-secondary" @click="addTask()">Add</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    valiMessage : false,
    title: "App",
    createTask: "",
    tasks: [],
    hideComplete: false,
  }),

  computed: {
    filterTask() {
      return this.hideComplete == true
        ? this.tasks.filter((v) => !v.done)
        : this.tasks;
    },
  },

  methods: {
    addTask() {
      if(this.createTask == null || this.createTask == '' ){
        this.valiMessage = true;
      }else{
        this.tasks.push({
        action: this.createTask,
        done: false,
      });
      localStorage.setItem("myStorage", JSON.stringify(this.tasks));

      this.createTask = '';
      this.valiMessage = false;
      }
      
    },
  },
  mounted() {
    let data = localStorage.getItem("myStorage");
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  },
};
</script>

<style>
.deleteTask {
  text-decoration: line-through;
}
</style>
