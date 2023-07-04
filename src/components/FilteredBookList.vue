<template>
  <div class="book-list" v-if="!searchMade && filteredBooks.length > 0">
    <p class="search-results">
      {{ filteredBooks.length }}
      {{ filteredBooks.length === 1 ? "result" : "results" }}
    </p>
    <div
      v-for="book in filteredBooks"
      :key="book.id"
      class="book-item"
      @click="openBookDetails(book)"
    >
      <div class="book-cover">
        <img :src="book.image" alt="Book Cover" class="book-cover-img" />
      </div>
      <div class="book-description">
        <h2 class="book-title">{{ book.title }}</h2>
        <h3 class="book-author">{{ book.author }}</h3>
        <h2 class="book-price">{{ book.price }}</h2>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "FilteredBookList",
  props: ["searchText", "filteredBooks", "searchMade"],
  methods: {
    openBookDetails(book) {
      this.$emit("open-book-details", book);
    },
  },
};
</script>

<style>
.book-list {
  margin-top: 20px;
}

.search-results {
  color: rgba(0, 0, 0, 0.55);
  font-size: 16px;
  line-height: 24px;
}

.book-item {
  display: flex;
  flex-direction: row;
  margin-bottom: 10px;
}

.book-item:hover {
  background-color: #f0f0f0;
}

.book-title {
  color: rgba(0, 0, 0, 0.95);
  font-family: "Inter";
  line-height: 24px;
  font-size: 16px;
  font-weight: 700;
  margin: 0;
}

.book-author {
  font-size: 14px;
  font-family: "Inter";
  font-weight: 400;
  line-height: 16.94px;
  color: rgba(0, 0, 0, 0.7);
  margin: 0;
}

.book-price {
  font-family: "Inter";
  color: rgba(0, 0, 0, 0.95);
  line-height: 16.94px;
  font-weight: 700;
  font-size: 14px;
  margin: 0;
}

.book-cover {
  width: 88px;
  height: 110px;
  padding-right: 5%;
}

.book-cover-img {
  width: 88px;
  height: 110px;
  border-radius: 4px;
}

.book-description {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
</style>
