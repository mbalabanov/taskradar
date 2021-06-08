<template>
  <div id="app">
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-8 offset-md-2">
          <h1 class="fw-lighter text-center">
            TASK
            <img alt="Vue logo" src="./assets/logo.png" width="32px" /> RADAR
          </h1>
          <WelcomeMessage msg="Create your task items on your to do list." />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import WelcomeMessage from "./components/taskradar.vue";

export default {
  name: "App",
  components: {
    WelcomeMessage,
  },
};
</script>

<style>
  .task-description {
      min-height: 10rem;
    }
</style>
