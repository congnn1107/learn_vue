<template>
  <TodoTask />
</template>

<script>
import TodoTask from './components/TodoTask.vue'

export default {
  name: 'App',
  components: {
    TodoTask,
}
}
</script>

<style>
#app {
  width: fit-content;
  min-height: calc(100vh - 150px);
  margin: auto;
  padding: 50px 75px;
  box-shadow: 0px 0px 3px gray;
}
</style>
