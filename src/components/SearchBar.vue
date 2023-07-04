<template>
  <div class="search-bar">
    <button class="search-icon">
      <img src="../assets/SearchIcon.svg" />
    </button>
    <input
      class="search-input"
      type="text"
      id="searchInput"
      :value="searchText"
      @input="$emit('update:searchText', $event.target.value)"
      placeholder="Search for a book"
    />
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  props: ["searchText"],
};
</script>

<style>
.search-bar {
  display: flex;
  width: 100%;
  max-width: 100%;
  border-radius: 4px;
  padding: 8px;
  align-items: center;
  border: 1px solid #dde1e3;
  gap: 10px;
}

@media (max-width: 680px) {
  .search-bar {
    width: 100%;
    max-width: 288px;
  }
}

.search-icon {
  width: 24px;
  height: 24px;
  background: none;
  border: none;
  cursor: pointer;
}

.search-input {
  flex: 1;
  height: 100%;
  width: 100%;
  border: none;
  background: none;
  outline: none;
}
</style>
