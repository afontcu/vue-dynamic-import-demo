<template>
  <div id="app">
    <component :is="componentConstructor" />

    <button v-for="i in [1,2,3]" :key="i" @click="exercise = i">
     Load exercise {{ i }}
    </button>
  </div>
</template>

<script>
/**
 * Let's assume a `./components` folder with three components on it:
 *   Exercise1.vue
 *   Exercise2.vue
 *   Exercise3.vue
 *
 * You would usually import all three and then register them.
 * We can avoid the hassle by leveraging dynamic import(), that
 * provide us with code splitting for free. ❤ webpack.
 */

export default {
  name: 'App',
  data() {
    return {
      exercise: 1, // Load first exercise by default
    };
  },
  computed: {
    componentConstructor() {
      // Little gotcha: We need to access to `this.exercise` outside of the
      // import function to make sure Vue's reactivity system works properly.
      const path = `./components/Exercise${this.exercise}`;

      // We need a template literal to make the dynamic import work!
      return () => import(`${path}`);
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
