<template>
  <div>
  <nav class="navbar navbar-expand-md navbar-light">
          <button class="navbar-toggler ml-auto mb-2 bg-light" type="button" data-toggle="collapse" data-target="#sidebar">
              <span class="navbar-toggle-icon"></span>
          </button>
      <Header class="header" ref="header" />
          <menu-layout class="menu"></menu-layout>
  </nav>
      <navigation class="naviGation"></navigation>
      <router-view />
  </div>
</template>

<script>

import Header from './header.vue'
import MenuLayout from './MenuLayout.vue'
import Navigation from './QuizNavigation.vue';


export default {
  components: {Header, MenuLayout, Navigation},
  data() {
  return {
  }
},
  created() {

  },
  mounted() {
  },
  computed: {},
  methods: {
   
   }
}
</script>
<style scoped>

.header {
  z-index: 999;

}

.naviGation {
  z-index: 2;
}

.menu {
  z-index: 1;
}
</style>