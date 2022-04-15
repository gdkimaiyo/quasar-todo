<template>
  <q-page class="bg-grey-3 column">
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="task in tasks"
        :key="task.id"
        clickable
        @click="task.done = !task.done"
        v-ripple
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
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Todo",

  setup() {
    return {
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
    markTask() {
      console.log(JSON.parse(JSON.stringify(this.tasks)));
    },
  },
});
</script>
