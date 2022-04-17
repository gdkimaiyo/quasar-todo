<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <div class="navigation">
        <div class="nav-menu q-pt-sm">
          <q-btn no-caps flat to="/" class="nav-btn" label="Home" />
          <q-btn no-caps flat to="/help" class="nav-btn" label="Help" />
        </div>

        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          class="menu-btn q-ma-sm"
          @click="drawer = !drawer"
        >
          <q-menu auto-close>
            <q-img
              class="absolute-top"
              src="src/assets/sky.png"
              style="height: 168px"
            >
              <div class="absolute-bottom bg-transparent text-center">
                <q-avatar size="72px" class="q-mb-sm">
                  <img src="src/assets/avatar.jpg" />
                </q-avatar>
                <div class="text-weight-bold">Gideon Kimaiyo</div>
                <div>@gideonGDK</div>
              </div>
            </q-img>
            <q-list
              style="
                min-width: 200px;
                height: calc(100% - 180px);
                margin-top: 180px;
              "
            >
              <q-item to="/" exact clickable v-ripple>
                <q-item-section avatar>
                  <q-icon name="list" />
                </q-item-section>

                <q-item-section> Home </q-item-section>
              </q-item>
              <q-item to="/help" exact clickable v-ripple>
                <q-item-section avatar>
                  <q-icon name="help" />
                </q-item-section>

                <q-item-section> Help </q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </q-btn>
      </div>
      <div class="q-px-md q-pt-lg q-mb-md header-content">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle1">{{ todaysDate }}</div>
      </div>
      <q-img src="src/assets/sky.png" class="header-image absolute-top" />
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { date } from "quasar";
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "MainLayout",

  setup() {
    return {
      drawer: ref(false),
    };
  },

  computed: {
    todaysDate() {
      const timeStamp = Date.now();
      return date.formatDate(timeStamp, "dddd, MMMM DD YYYY");
    },
  },
});
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.3;
  filter: grayscale(100%);
}
.navigation {
  position: absolute;
  right: 0;
}
.nav-menu {
  display: block;
  margin-right: 48px;
}
.menu-btn {
  display: none;
}
.nav-btn:hover {
  color: #42b983;
}
.header-content {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  margin-top: 48px;
  margin-bottom: 24px;
}

@media only screen and (max-width: 575px) {
  .menu-btn {
    display: block;
  }
  .nav-menu {
    display: none;
  }
}
</style>
