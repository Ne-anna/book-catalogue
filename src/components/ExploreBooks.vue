<template>
  <div v-if="filteredBooks.length === 0" class="explore-books">
    <h1 class="explore-books-text">Explore Books</h1>
    <img class="explore-books-logo" src="../assets/Book.png" alt="Book Logo" />
  </div>
</template>

<script>
export default {
  name: "ExploreBooks",
  props: {
    filteredBooks: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style>
.explore-books-text {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-family: "Inter";
  font-weight: 700;
  font-size: 24px;
  line-height: 29.05px;
}

.explore-books-logo {
  display: block;
  margin: 0 auto;
}
</style>
