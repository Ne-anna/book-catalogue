<template>
  <div class="landing-view">
    <div class="landing-view-container">
      <SearchBar
        :searchText="searchText"
        @update:searchText="updateSearchText"
        @search="searchBooks"
      />
      <ExploreBooks :filteredBooks="filteredBooks" />
      <FilteredBookList
        :searchText="searchText"
        :filteredBooks="filteredBooks"
        :searchMade="searchMade"
        @open-book-details="openBookDetails"
      />
      <BookDetailedInfo
        :book="selectedBook"
        v-if="selectedBook"
        @close-book-details="resetSelectedBook"
      />
      <div
        v-if="filteredBooks.length === 0 && searchText !== ''"
        class="failed-search-message"
      >
        <p>Well, the book you've been searching for doesn't exist.</p>
      </div>
    </div>
  </div>
</template>

<script>
import SearchBar from "../components/SearchBar.vue";
import ExploreBooks from "../components/ExploreBooks.vue";
import FilteredBookList from "../components/FilteredBookList.vue";
import BookDetailedInfo from "../components/BookDetailedInfo.vue";

export default {
  name: "LandingView",
  components: {
    SearchBar,
    ExploreBooks,
    FilteredBookList,
    BookDetailedInfo,
  },
  data() {
    return {
      books: [],
      searchText: "",
      searchMade: false,
      selectedBook: null,
    };
  },
  mounted() {
    fetch("http://localhost:3000/books")
      .then((response) => response.json())
      .then((books) => (this.books = books))
      .catch((err) => console.log(err.message));
  },
  computed: {
    filteredBooks() {
      const search = this.searchText.toLowerCase().trim();
      if (search === "") {
        return [];
      } else {
        const filtered = this.books.filter((book) => {
          const title = book.title.toLowerCase();
          const author = book.author.toLowerCase();
          const isbn = book.isbn;
          return (
            title.includes(search) ||
            author.includes(search) ||
            isbn.includes(search)
          );
        });
        return filtered;
      }
    },
  },
  methods: {
    updateSearchText(value) {
      this.searchText = value;
    },
    searchBooks() {
      const search = this.searchText.toLowerCase().trim();
      const match = this.books.some((book) => {
        const title = book.title.toLowerCase();
        return title.includes(search);
      });
      this.searchMade = match;
    },
    openBookDetails(book) {
      this.selectedBook = book;
    },
    resetSelectedBook() {
      this.selectedBook = null;
    },
  },
};
</script>

<style>
.landing-view {
  display: grid;
  width: 320px;
  margin-left: auto;
  margin-right: auto;
  padding-top: 32px;
}

@media (min-width: 745px) {
  .landing-view {
    width: 744px;
  }
}

@media (min-width: 1025px) {
  .landing-view {
    width: 1024px;
  }
}

@media (min-width: 1281px) {
  .landing-view {
    width: 1280px;
  }
}

.landing-view-container {
  display: flex;
  flex-direction: column;
  gap: 32px;
  flex-wrap: wrap;
}

.failed-search-message {
  margin-top: 20px;
  text-align: center;
  color: gray;
}
</style>
