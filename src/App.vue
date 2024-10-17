<template>
  <div id="App">
    <div class="container mt-5">
      <div class="row justify-content-md-center">
        <div class="col-md-8 bg-dark p-3 text-light">
          <div class="d-flex justify-content-between">
            <div class="lead fw-bold">{{ name }} To-Do List</div>
            <div>
              <input type="checkbox" v-model="hide" class="form-check-input" />
              <label class="form-check-label px-2">HideTaskCompleted</label>
            </div>
          </div>
          <hr class="text-light" />
          <div class="row py-2">
            <div class="col-12 col-md-2">
              <button
                class="btn btn-success text-align-center mt-3 p-2"
                @click="addNewTask"
              >
                AddTask
              </button>
            </div>
            <div class="col align-self-center border-none p-3">
              <input class="form-control w-100 p-2" v-model="newItemText" />
            </div>
          </div>

          <div
            class="col d-flex justify-content-between px-3 py-2"
            v-for="task in hiddenList"
            :key="task.id"
          >
            <div># {{ task.title }}</div>
            <div>
              <input
                type="checkbox"
                v-model="task.completed"
                class="form-check-input"
              />
            </div>
          </div>
          <div class="col d-flex justify-content-center m-2">
            <button class="btn btn-primary m-2 p-2" @click="clearList">
              Clear
            </button>
            <button class="btn btn-danger m-2 p-2" @click="clearAll">
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      name: "October's",
      tasks: [],
      hide: false,
      newItemText: "",
    };
  },
  computed: {
    hiddenList() {
      return this.hide
        ? this.tasks.filter((task) => !task.completed)
        : this.tasks;
    },
  },
  methods: {
    addNewTask() {
      this.tasks.push({
        id: Date.now(),
        title: this.newItemText,
        completed: false,
      });
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
      this.newItemText = "";
    },
    clearList() {
      this.tasks = this.tasks.filter((task) => !task.completed);
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    clearAll() {
      this.tasks = [];
    },
  },
  created() {
    let data = localStorage.getItem("tasks");
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  },
};
</script>

<style lang="scss">
body {
  background-color: #f8f8f8 !important;
}
</style>
