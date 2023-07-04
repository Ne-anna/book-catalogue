<template>
  <div class="book-detailed-info">
    <button class="button-explore" @click="closeBookDetails">
      <img src="../assets/arrow.svg" />
      <p>
        <u class="underline"> Explore </u>
      </p>
    </button>

    <div class="book-container">
      <div class="book-item">
        <div class="book-cover">
          <img type="image" :src="book.image" class="book-cover-img" />
        </div>
        <div class="book-description">
          <h2 class="book-title">{{ book.title }}</h2>
          <h3 class="book-author">{{ book.author }}</h3>
          <h2 class="book-price">{{ book.price }}</h2>
          <h2 class="book-publisher">{{ book.publisher }}</h2>
        </div>
      </div>

      <div class="book-rating">
        <h1 class="book-numeral-rating">{{ book.rating }}</h1>
        <div class="book-star-rating">
          <span v-for="index in 5" :key="index" class="stars">
            {{ index <= book.rating ? "★" : "☆" }}
          </span>
        </div>
      </div>
      <h2 class="book-description">{{ book.description }}</h2>
    </div>
    <div class="review-container">
      <div class="book-review">
        <h3>Write a review</h3>
        <h4>Rate the book</h4>
        <div class="book-review-star-rating">
          <span
            v-for="index in 5"
            :key="index"
            :class="{ selected: index <= selectedRating }"
            class="stars"
            @click="selectRating(index)"
            @mouseleave="resetRating()"
          >
            {{ index <= selectedRating ? "★" : "☆" }}
          </span>
        </div>
        <textarea
          class="review-text-input"
          type="text"
          v-model="reviewText"
        ></textarea>
        <div class="container-submit">
          <button
            class="submit-review-button"
            @click="submitReview"
            :disabled="!isReviewValid"
            v-if="!showSuccessMessage && !showErrorMessage"
          >
            Submit review
          </button>
        </div>
      </div>
    </div>
    <div class="banner-container">
      <div class="success-banner" v-if="showSuccessMessage">
        <p class="success-banner-text">Thank you! Review received.</p>
      </div>
      <div class="error-banner" v-if="showErrorMessage">
        <p class="success-banner-text">
          There has been a problem. Please try again.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "BookDetailedInfo",
  props: ["book", "openBookDetails"],
  data() {
    return {
      selectedRating: 0,
      hoveredRating: 0,
      reviewText: "",
      showSuccessMessage: false,
      showErrorMessage: false,
    };
  },
  methods: {
    closeBookDetails() {
      this.$emit("close-book-details");
    },
    selectRating(rating) {
      this.selectedRating = rating;
    },
    resetRating() {
      this.hoveredRating = 0;
    },
    submitReview() {
      if (!this.isReviewValid) {
        return;
      }

      const reviewData = {
        rating: this.selectedRating,
        review: this.reviewText,
        reviewedBook: this.book.title,
        bookId: this.book.id,
        bookIsbn: this.book.isbn,
      };
      axios
        .post("https://enwfsq1f8x2s.x.pipedream.net/", reviewData)
        .then((response) => {
          this.showSuccessMessage = true;
          this.showErrorMessage = false;
          this.selectedRating = 0;
          this.reviewText = "";
          console.log(response);
        })
        .catch((error) => {
          this.showSuccessMessage = false;
          this.showErrorMessage = true;
          console.log(error);
        });
      this.selectedRating = 0;
      this.reviewText = "";
    },
  },
  computed: {
    isReviewValid() {
      return this.selectedRating > 0 && this.reviewText.trim() !== "";
    },
  },
};
</script>

<style>
.book-detailed-info {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgb(252, 252, 252);
  z-index: 999;
}

.book-container,
.review-container,
.banner-container {
  padding: 0 16px;
}

.button-explore {
  display: flex;
  align-items: center;
  padding: 0;
  border: none;
  background: none;
  padding-left: 8px;
}

.button-explore img {
  width: 24px;
  height: 24px;
}

.underline {
  font-size: 1.1em;
  font-weight: bold;
}

.book-publisher {
  color: rgba(0, 0, 0, 0.7);
  font-size: 16px;
  margin: 0;
  font-family: "Inter";
}

.book-rating {
  display: flex;
  align-items: center;
  gap: 4px;
}

.book-numeral-rating {
  margin-right: 10px;
  font-family: "Inter";
  font-weight: 400;
  line-height: 24px;
  font-size: 16px;
  color: rgba(0, 0, 0 0.55);
}

.book-star-rating {
  display: inline-block;
  padding-bottom: 4px;
}

.book-description {
  font-family: "Inter";
  font-weight: 400;
  line-height: 24px;
  font-size: 16px;
  color: rgba(0, 0, 0 0.7);
}

.book-review {
  padding-top: 6px;
}

.book-review h3 {
  font-family: "Inter";
  font-weight: 700;
  line-height: 29.05px;
}

.book-review h4 {
  font-family: "Inter";
  font-weight: 700;
  margin-bottom: 0;
}

.stars {
  display: inline-block;
  color: gold;
  font-size: 24px;
  position: relative;
}
.stars.selected {
  color: gold;
}

.review-text-input {
  border: 1px solid #dde1e3;
  border-radius: 4px;
  height: 200px;
  width: 100%;
  outline: none;
}

.container-submit {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 6%;
}

.submit-review-button {
  border-radius: 24px;
  border: 2px solid rgba(57, 128, 178, 1);
  gap: 10px;
  width: 95%;
  padding: 10px;
  color: rgb(98, 156, 197);
}

.submit-review-button:hover {
  background-color: rgb(215, 225, 233);
}

.success-banner,
.error-banner {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  background-color: rgba(207, 235, 255, 1);
  font-family: "Inter";
  border-radius: 4px;
  width: 80%;
}

.success-banner-text {
  font-weight: 700;
  padding-left: 3%;
}

@media (min-width: 1024px) {
  .container-submit {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    margin-top: 1%;
  }

  .submit-review-button {
    width: 10%;
  }

  .success-banner,
  .error-banner {
    width: 20%;
  }
}
</style>
