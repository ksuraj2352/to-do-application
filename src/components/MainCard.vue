<template>
  <v-row>
    <v-spacer></v-spacer>

    <v-col md="" cols="12">
      <v-card class="ma-4 mt-9">
        <v-card-title class="d-flex justify-center">
          <h3>To-Do-Task</h3>
        </v-card-title>

        <v-divider class="mx-10"></v-divider>

        <v-text-field
          v-model.trim="message"
          :append-icon="marker ? 'mdi-plus' : null"
          class="mx-6 mt-5"
          filled
          clear-icon="mdi-close-circle"
          clearable
          rounded
          label="Add Task"
          type="text"
          @click:append="addTask"
          @click:clear="clearMessage"
          @keyup.enter="addTask"
          @blur="clearValidation()"
        ></v-text-field>

        <v-alert v-if="error" dense outlined type="error" class="mx-6">
          Please Enter A <strong>Valid Task</strong>.
        </v-alert>

        <v-card-text>
          <div v-if="numberOfTasks > 0">
            <v-chip class="ma-2" color="primary" outlined pill>
              Pending Tasks : -
              <!-- <v-icon right> mdi-account-outline </v-icon> -->
              <!-- <span class="mx-3">{{ pendingTasks }}</span> -->
              <v-avatar right class="orange lighten-3 mr-1">
                {{ pendingTasks }}
              </v-avatar>
            </v-chip>
            <v-list>
              <task-item
                v-for="(task, index) in tasks"
                :key="task.id"
                :task="task"
                @toggleTaskStatus="changeTaskStatus(task)"
                @remove="deleteTask(index)"
              ></task-item>
            </v-list>
          </div>
          <div v-else>
            <h3 class="d-flex justify-center">
              Hurray, You Have Completed All Your Tasks
            </h3>
          </div>
        </v-card-text>

        <div class="text-center">
          <v-btn
            @click="clearCompletedTask"
            class="mt-1 mx-4 mb-5"
            outlined
            color="indigo"
          >
            Clear Completed
          </v-btn>
          <v-btn
            @click="clearAllTask"
            class="mt-1 mx-4 mb-5"
            rounded
            color="primary"
            dark
          >
            Clear All
          </v-btn>
        </div>
      </v-card>
    </v-col>
    <v-spacer></v-spacer>
  </v-row>
</template>

<script>
import TaskItem from "./taskItem.vue";
export default {
  components: { TaskItem },
  data() {
    return {
      message: "",
      marker: true,
      error: false,
      tasks: [
        {
          id: 1,
          taskName: "Learn Vue js",
          isCompleted: false,
        },
        {
          id: 2,
          taskName: "Learn Node Js",
          isCompleted: false,
        },
        {
          id: 3,
          taskName: "Learn HTML CSS",
          isCompleted: true,
        },
        {
          id: 4,
          taskName: "Learn Javascript",
          isCompleted: true,
        },
      ],
    };
  },

  computed: {
    icon() {
      return this.icons[this.iconIndex];
    },
    numberOfTasks() {
      return this.tasks.length;
    },
    pendingTasks() {
      const pendingTasksArray = this.tasks.filter(
        (task) => task.isCompleted === false
      );
      return pendingTasksArray.length;
    },
  },

  methods: {
    changeTaskStatus(task) {
      task.isCompleted = !task.isCompleted;
    },
    deleteTask(id) {
      this.tasks.splice(id, 1);
      console.log(id);
    },
    clearValidation() {
      this.error = false
    },
    clearAllTask() {
      this.tasks = [];
    },
    clearCompletedTask() {
      const completedTasks = this.tasks.filter(function (task) {
        return task.isCompleted === false;
      });
      console.log(completedTasks);
      this.tasks = completedTasks;
    },
    addTask() {
      if (this.message !== "") {
        this.marker = !this.marker;
      }

      if (this.message === "") {
        return this.error = true;
      } else {
        const taskPass = {
          id: Date.now() + Math.random(),
          taskName: this.message,
          isCompleted: false,
        };
        this.tasks.push(taskPass);
        this.marker = true;
        this.clearMessage();
        this.error = false
      }
    },
    sendMessage() {
      this.resetIcon();
      this.clearMessage();
    },
    clearMessage() {
      this.message = "";
    },
  },
};
</script>

<style lang="scss" scoped>
.tab-index {
  position: relative;
  z-index: 1;
}
</style>
