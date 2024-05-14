<template>
  <div id="app">
    <Header @set-view="currentView = $event" />
    <component :is="currentViewComponent"></component>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Posts from './components/Posts.vue';
import Todos from './components/Todos.vue';

export default {
  components: {
    Header
  },
  data() {
    return {
      currentView: 'posts'
    };
  },
  computed: {
    currentViewComponent() {
      return this.currentView === 'posts' ? Posts : Todos;
    }
  }
}
</script>

<style>

</style>
