<template>
  <div class="hello">
    <h3 class="fw-lighter text-center">{{ msg }}</h3>
    <p class="text-center">
      Create, edit or delete your tasks, click on status to change to "Doing" or
      "Done".
    </p>
    <div class="input-group my-5">
      <input
        type="text"
        v-model="task"
        placeholder="Enter your task"
        class="form-control form-control-lg"
        aria-describedby="save-task"
        @keyup.enter="saveTask"
        id="taskInput"
        ref="taskInput"
      />
      <button class="btn btn-primary" @click="saveTask">Save task</button>
    </div>
    <div class="row alert alert-light">
      <div class="col-md-3">
        <h3 class="fw-lighter text-center">Total Tasks: <span class="badge bg-dark">{{ countTasks() }}</span></h3>
      </div>
      <div class="col-md-3">
        <h3 class="fw-lighter text-center">Open: <span class="badge bg-secondary">{{ countOpenTasks() }}</span></h3>
      </div>
      <div class="col-md-3">
        <h3 class="fw-lighter text-center">Doing: <span class="badge bg-warning">{{ countDoingTasks() }}</span></h3>
      </div>
      <div class="col-md-3">
        <h3 class="fw-lighter text-center">Done: <span class="badge bg-success">{{ countDoneTasks() }}</span></h3>
      </div>
    </div>
    <div class="row">
      <div v-for="(task, index) in tasks" :key="index" class="col-md-6">
        <div class="card m-1 p-2">
          <h4 class="card-title fw-lighter">Task {{ index + 1 }}</h4>
          <div class="fs-5 fw-lighter task-description border-bottom">
            <span
              :class="{
                'text-decoration-line-through': task.statusList === 'done',
              }"
            >
              {{ task.name }}
            </span>
          </div>
          <div class="row mt-4">
            <div class="col-md-6">
              <span
                class="pointer noselect"
                @click="statusChange(index)"
                :class="{
                  'alert alert-secondary': task.statusList === 'open',
                  'alert alert-success': task.statusList === 'done',
                  'alert alert-warning text-dark': task.statusList === 'doing',
                }"
              >
                {{ formatStatus(task.statusList) }}
              </span>
            </div>
            <div class="col-md-6">
              <a @click="editTask(index)" class="btn btn-primary btn-sm m-1">
                Edit task
              </a>
              <a @click="deleteTask(index)" class="btn btn-danger btn-sm m-1">
                Delete task
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Taskradar", // Rename taskradar.vue to "Taskradar" (capitalized)
  props: {
    msg: String,
  },

/* 
  Make Task ITEMS separate COMPONENTS.
  Make Task LISTS separate COMPONENTS.
  Split up the code into separate files (e.g. each button with settings).
*/

  data() {
    return {
      task: "",
      editedTask: null,
      statusList: ["open", "doing", "done"],

      tasks: [
        {
          name: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.",
          statusList: "open",
        },
        {
          name: "At vero eos et accusam et justo duo dolores et ea rebum.",
          statusList: "doing",
        },
        {
          name: "Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.",
          statusList: "open",
        },
        {
          name: "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.",
          statusList: "done",
        },
      ],
    };
  },

  methods: {
    saveTask() {
      if (this.task.length === 0) return;
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        this.tasks.push({
          name: this.task,
          statusList: "open",
        });
      }
      this.task = "";
    },

    // DRY!!! One single counter function but used in different function which return 

    countTasks() {
      return this.tasks.length;
    },

    countOpenTasks() {
      let openTaskCount = 0;
      for(let singleTask of this.tasks) {
        if(singleTask.statusList === 'open') {
          openTaskCount++;
        }
      }
      return openTaskCount;
    },

    countDoingTasks() {
      let doingTaskCount = 0;
      for(let singleTask of this.tasks) {
        if(singleTask.statusList === 'doing') {
          doingTaskCount++;
        }
      }
      return doingTaskCount;
    },

    countDoneTasks() {
      let doneTaskCount = 0;
      for(let singleTask of this.tasks) {
        if(singleTask.statusList === 'done') {
          doneTaskCount++;
        }
      }
      return doneTaskCount;
    },

    statusChange(index) {
      let newIterator = this.statusList.indexOf(this.tasks[index].statusList);
      if (++newIterator > 2) newIterator = 0; // Use curly braces and longer notation for clarity 
      this.tasks[index].statusList = this.statusList[newIterator];
    },

    formatStatus(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
      window.scrollTo(0, 0);
      this.$refs.taskInput.focus();
    },
  },
};
</script>
