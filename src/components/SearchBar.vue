<template>
  <div class="searchBarContainer">
    <b-form-input
      v-model="searchInput"
      placeholder="Search drug inventory"
      style="width: 40%; border: 1px solid black"
      v-on:input="handleSearchInput"
    ></b-form-input>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  data: function () {
    return { searchInput: "" };
  },
  methods: {
    handleSearchInput: function (e) {
      this.$emit("supply-search", e);
    },
  },
};
</script>

<style>
.searchBarContainer {
  display: flex;
  width: 100%;
  margin-top: 3%;
  align-items: center;
  justify-content: center;
}
</style>
