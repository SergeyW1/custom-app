<template>
  <div class="app">
    <user-form />
  </div>
</template>

<script>
import UserForm from "./Components/UserForm.vue";
export default {
  components: { UserForm },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  margin: 0 auto;
  width: 1100px;
  padding: 20px;
  font-family: "Comfortaa", cursive;
}
</style>
