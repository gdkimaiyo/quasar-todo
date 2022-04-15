<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm">
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
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="task in tasks"
        :key="task.id"
        v-ripple
        clickable
        @click="task.done = !task.done"
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
    <div class="no-tasks absolute-center" v-if="tasks.length === 0">
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
      tasks: ref([
        {
          id: Math.floor(Math.random() * 100) * Date.now(),
          title: "Learn JavaScript",
          done: true,
        },
        {
          id: Math.floor(Math.random() * 100) * Date.now(),
          title: "Learn Vue",
          done: true,
        },
        {
          id: Math.floor(Math.random() * 100) * Date.now(),
          title: "Use Quasar Framework with Vue",
          done: false,
        },
      ]),
    };
  },

  methods: {
    markTask(taskId) {
      this.tasks.forEach((task) => {
        if (task.id === taskId) {
          task.done = !task.done;
        }
      });
      console.log(JSON.parse(JSON.stringify(this.tasks)));
    },

    addTask() {
      this.tasks.push({
        id: Math.floor(Math.random() * 100) * Date.now(),
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },

    deleteTask(taskId) {
      Dialog.create({
        title: "Confirm",
        message: "Are you sure you want to delete task?",
        cancel: true,
        persistent: true,
      }).onOk(() => {
        this.tasks = this.tasks.filter((task) => task.id !== taskId);
        Notify.create({
          type: "positive",
          message: "Task successfully deleted",
          group: false,
        });
      });
    },
  },
});
</script>

<style lang="scss" scoped>
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
