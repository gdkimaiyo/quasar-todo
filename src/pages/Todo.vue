<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm q-py-md input-section">
      <q-input
        filled
        v-model="newTask"
        placeholder="Add task"
        bg-color="white"
        class="col"
        dense
        @keyup.enter="addTask"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="q-pa-sm tasks-section" separator>
      <q-item
        v-for="task in tasks"
        :key="task.id"
        v-ripple
        clickable
        @click="markTask(task.id)"
        class="bg-white"
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            color="primary"
            @click.stop="deleteTask(task.id)"
            dense
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks text-center q-mt-xl" v-if="tasks?.length === 0">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-center text-primary">No Tasks...</div>
    </div>
  </q-page>
</template>

<script>
import { Dialog, Notify } from "quasar";
import { defineComponent, ref } from "vue";

export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Todo",

  setup() {
    return {
      newTask: ref(""),
      tasks: ref([]),
    };
  },

  methods: {
    markTask(taskId) {
      this.markTodoAsDOne(taskId);
    },

    addTask() {
      if (this.newTask !== "") {
        const newTask = {
          id: Math.floor(Math.random() * 100) * Date.now(),
          title: this.newTask,
          done: false,
        };
        this.addTodos(newTask);
        this.getTodos();
        this.newTask = "";
      }
    },

    deleteTask(taskId) {
      Dialog.create({
        title: "Confirm",
        message: "Are you sure you want to delete task?",
        cancel: true,
        persistent: true,
      }).onOk(() => {
        this.deleteTodos(taskId);
      });
    },

    /*
     * Local Storage
     */
    // Get existing tasks saved in local storage
    getTodos() {
      if (
        localStorage.getItem("quasar_todos") &&
        typeof Storage !== undefined
      ) {
        this.tasks = JSON.parse(localStorage.getItem("quasar_todos") || "[]");
        // console.log(JSON.parse(JSON.stringify(this.tasks)));
      } else {
        this.tasks = [];
      }
    },

    markTodoAsDOne(taskId) {
      if (
        localStorage.getItem("quasar_todos") &&
        typeof Storage !== undefined
      ) {
        let todos = JSON.parse(localStorage.getItem("quasar_todos") || "[]");
        todos.forEach((task) => {
          if (task.id === taskId) {
            task.done = !task.done;
          }
        });
        localStorage.setItem("quasar_todos", JSON.stringify(todos));
        this.getTodos();
      } else {
        Notify.create({
          type: "negative",
          message: "Error! Unable to mark task as done",
          group: false,
        });
      }
    },

    // Save a task to local storage
    addTodos(task) {
      if (
        localStorage.getItem("quasar_todos") &&
        typeof Storage !== undefined
      ) {
        let todos = JSON.parse(localStorage.getItem("quasar_todos") || "[]");
        todos.push(task);
        localStorage.setItem("quasar_todos", JSON.stringify(todos));
      } else {
        if (typeof Storage !== undefined) {
          let todos = [];
          todos.push(task);
          localStorage.setItem("quasar_todos", JSON.stringify(todos));
        } else {
          Notify.create({
            type: "negative",
            message: "Error! Unable to add task",
            group: false,
          });
        }
      }
    },

    // Delete a task in local storage
    deleteTodos(taskId) {
      let todos = JSON.parse(localStorage.getItem("quasar_todos") || "[]");
      todos = todos.filter((task) => task.id !== taskId);
      localStorage.setItem("quasar_todos", JSON.stringify(todos));
      Notify.create({
        type: "positive",
        message: "Task successfully deleted",
        group: false,
      });
      this.getTodos();
    },
  },

  mounted() {
    this.getTodos();
  },
});
</script>

<style lang="scss" scoped>
.input-section,
.tasks-section {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}
.done {
  .q-item__label {
    color: #bbb;
    text-decoration: line-through;
  }
}
.no-tasks {
  opacity: 0.8;
}
</style>
