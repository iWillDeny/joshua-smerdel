<template>
  <div class="main">
    <h2>Product Reviews for {{ name }}</h2>

    <p class="description">{{ description }}</p>

    <a v-on:click.prevent="showForm" 
      v-if="formVisible === false">Show Form</a>
    <a v-on:click.prevent="clearForm" 
      v-if="formVisible">Hide Form</a>

    <form v-on:submit.prevent="addReview"
      v-if="formVisible"
    >
      <div class="form-element">
        <label for="reviewer">Name:</label>
        <input type="text" v-model="newReview.reviewer">
      </div>
      <div class="form-element">
        <label for="title">Title:</label>
        <input type="text" v-model="newReview.title">
      </div>
      <div class="form-element">
        <label for="rating">Rating:</label>
        <select v-model="newReview.rating">
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <option>4</option>
          <option>5</option>
        </select>
      </div>
      <div class="form-element">
        <label for="review">Review:</label>
        <textarea v-model="newReview.review"></textarea>
      </div>

      <input type="submit" value="Save">
      <input type="button" v-on:click="clearForm" value="Cancel">
    </form>

    <div class="well-display">
      <div class="well" v-on:click="setFilter(0)">
        <span class="amount">{{ averageRating }}</span>
        Average Rating
      </div>

      <div class="well" v-on:click="setFilter(1)">
        <span class="amount">{{ numberOfOneStarReviews }}</span>
        1 Star Review{{ numberOfOneStarReviews === 1 ? '' : 's' }}
      </div>

      <div class="well" v-on:click="setFilter(2)">
        <span class="amount">{{ numberOfTwoStarReviews }}</span>
        2 Star Review{{ numberOfTwoStarReviews === 1 ? '' : 's' }}
      </div>

      <div class="well" v-on:click="setFilter(3)">
        <span class="amount">{{ numberOfThreeStarReviews }}</span>
        3 Star Review{{ numberOfThreeStarReviews === 1 ? '' : 's' }}
      </div>

      <div class="well" v-on:click="setFilter(4)">
        <span class="amount">{{ numberOfFourStarReviews }}</span>
        4 Star Review{{ numberOfFourStarReviews === 1 ? '' : 's' }}
      </div>

      <div class="well" v-on:click="setFilter(5)">
        <span class="amount">{{ numberOfFiveStarReviews }}</span>
        5 Star Review{{ numberOfFiveStarReviews === 1 ? '' : 's' }}
      </div>
    </div>

    <div
      class="review"
      v-bind:class="{ favorited: review.favorited }"
      v-for="review in filteredReviews"
      v-bind:key="review.id"
    >
      <h4>{{ review.reviewer }}</h4>
      <div class="rating">
        <img
          src="../assets/star.png"
          v-bind:title="review.rating + ' Star Review'"
          class="ratingStar"
          v-for="n in review.rating"
          v-bind:key="n"
        />
      </div>
      <h3>{{ review.title }}</h3>

      <p>{{ review.review }}</p>

      <p>
        Favorite?
        <input type="checkbox" v-model="review.favorited" />
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "product-review",
  data() {
    // this is what the "object" knows
    // the state of the object
    return {
      name: "Cigar Parties for Dummies",
      description: "Host and plan the perfect cigar party for all of your squirrelly friends.",
      formVisible: false,
      filter: 0,
      newReview: {},
      reviews: [
        {
          reviewer: "Malcolm Gladwell",
          title: "What a book!",
          review:
            "It certainly is a book. I mean, I can see that. Pages kept together with glue and there's writing on it, in some language.",
          rating: 3,
          favorited: false
        },
        {
          reviewer: "Tim Ferriss",
          title: "Had a cigar party started in less than 4 hours.",
          review:
            "It should have been called the four hour cigar party. That's amazing. I have a new idea for muse because of this.",
          rating: 4,
          favorited: false
        },
        {
          reviewer: "Ramit Sethi",
          title: "What every new entrepreneurs needs. A door stop.",
          review:
            "When I sell my courses, I'm always telling people that if a book costs less than $20, they should just buy it. If they only learn one thing from it, it was worth it. Wish I learned something from this book.",
          rating: 1,
          favorited: false
        },
        {
          reviewer: "Gary Vaynerchuk",
          title: "And I thought I could write",
          review:
            "There are a lot of good, solid tips in this book. I don't want to ruin it, but prelighting all the cigars is worth the price of admission alone.",
          rating: 3,
          favorited: false
        }
      ]
    };
  },
  computed: {
    averageRating() {
      let sum = this.reviews.reduce((currentSum, review) => {
        return currentSum + review.rating;
      }, 0);
      return (sum / this.reviews.length).toFixed(2);
    },
    numberOfOneStarReviews() {
      return this.numberOfReviews(1);
    },
    numberOfTwoStarReviews() {
      return this.numberOfReviews(2);
    },
    numberOfThreeStarReviews() {
      return this.numberOfReviews(3);
    },
    numberOfFourStarReviews() {
      return this.numberOfReviews(4);
    },
    numberOfFiveStarReviews() {
      return this.numberOfReviews(5);
    },
    filteredReviews() {

      return this.reviews.filter(review => this.filter === 0 ? true : review.rating === this.filter);

      // return this.reviews.filter(review => {
      //   if(this.filter === 0){
      //     return true;
      //   }

      //   return review.rating === this.filter;

      // });
    }
  },
  methods: {
    // what can it do?
    addReview(){
      // what do we do here?
      this.newReview.rating = parseInt(this.newReview.rating);
      this.reviews.unshift(this.newReview); 
      this.clearForm()
    },
    clearForm(){
      this.newReview = {};
      this.formVisible = false;
    },
    showForm(){
      this.formVisible = true;
    },
    setFilter(value) {
      this.filter = value;
    },
    numberOfReviews(stars) {
      return this.reviews.filter(review => review.rating === stars).length;
    }
  }
};
</script>

<style scoped>
div.main {
  margin: 1rem 0;
}

div.main div.well-display {
  display: flex;
  justify-content: space-around;
}

div.main div.well-display div.well {
  display: inline-block;
  width: 15%;
  border: 1px black solid;
  border-radius: 6px;
  text-align: center;
  margin: 0.25rem;
}

div.main div.well-display div.well span.amount {
  color: darkslategray;
  display: block;
  font-size: 2.5rem;
}

div.main div.review {
  border: 1px black solid;
  border-radius: 6px;
  padding: 1rem;
  margin: 10px;
}

div.main div.review.favorited {
  background-color: lightyellow;
}

div.main div.review div.rating {
  height: 2rem;
  display: inline-block;
  vertical-align: top;
  margin: 0 0.5rem;
}

div.main div.review div.rating img {
  height: 100%;
}

div.main div.review p {
  margin: 20px;
}

div.main div.review h3 {
  display: inline-block;
}

div.main div.review h4 {
  font-size: 1rem;
}

div.form-element {
  margin-top: 10px;
}
div.form-element > label {
  display: block;
}
div.form-element > input, div.form-element > select {
  height: 30px;
  width: 300px;
}
div.form-element > textarea {
  height: 60px;
  width: 300px;
}
form > input[type=button] {
  width: 100px;
}
form > input[type=submit] {
  width: 100px;
  margin-right: 10px;
}

.well, .well span {
  cursor: pointer;
}

.well:hover {
  background-color: lightgoldenrodyellow;
}


</style>

