<template>
  <div class="header">
    <img src="../assets/logo.png" height="4%" width="4%" />
    <h3>{{ title }}</h3>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: String,
  },
};
</script>

<style>
.header {
  width: 100%;
  display: "flex";
  flex-direction: "row";
}
</style>
