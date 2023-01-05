<script>
import Home from '@/components/Home.vue'
import Quiz from "@/components/Quiz.vue";
import Wanted from "@/components/Wanted.vue";

const routes = {
  '/': Home,
  '/quiz': Quiz,
  '/wanted': Wanted,
}

export default {
  data() {
    return {
      currentPath: window.location.hash
    }
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || '/'] || NotFound
    }
  },
  mounted() {
    window.addEventListener('hashchange', () => {
      this.currentPath = window.location.hash
    })
  }
}
</script>

<template>
  <a href="#/">Home</a> |
  <a href="#/quiz">Quiz</a> |
  <a href="#/wanted">Wanted</a> |
  <component :is="currentView" />
</template>