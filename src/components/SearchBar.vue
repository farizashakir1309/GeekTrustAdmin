<template>
  <div class="search-bar">
    <input
      v-model="searchValStr"
      class="search-input"
      placeholder="Enter Program Id/Title or Chron ID or Friendly URL"
      @keyup.enter="doSearch"
    />
  </div>
</template>
<script>
export default {
  name: "SearchBar",
  data() {
    return {
      searchValStr: "",
    };
  },
  methods: {
    doSearch() {
      this.$emit("search", this.searchValStr);
    },
  },
};
</script>
<style scoped>
.search-bar {
  display: flex;
  align-items: center;
  margin: 0 auto;
}

.search-input {
  border: 1px solid #ccc;
  padding: 6px 10px;
  font-size: 14px;
  flex: 1;
}

.search-button,
.clear-button {
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
}
</style>