<template>
  <div id="app">
    <todo-container></todo-container>
  </div>
</template>

<script>
import TodoContainer from "./components/TodoContainer";

export default {
  name: "app",
  components: { TodoContainer }
};
</script>
