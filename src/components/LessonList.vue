<template>
  <div>
    <link rel="stylesheet" href="style.css" />
    <div class="searchBox" id="searchField">
      <p>
        <input
          type="text"
          class="searchInput"
          v-model="searchTerm"
          placeholder="Search"
          aria-label="Search"
        />
        <img src="images/search.gif" width="40" height="40" />
      </p>
    </div>

    <div class="condSorting">
      <p>
        <input
          type="radio"
          id="priceLowHigh"
          value="priceLowHigh"
          v-model="sortStyle"
        />
        <label for="priceLowHigh">Price: Low to High</label>
      </p>
      <p>
        <input
          type="radio"
          id="priceHighLow"
          value="priceHighLow"
          v-model="sortStyle"
        />
        <label for="priceHighLow">Price : High to Low</label>
      </p>
      <p>
        <input type="radio" id="aToZ" value="aToZ" v-model="sortStyle" />
        <label for="aToZ">Subject: A to Z</label>
      </p>
      <p>
        <input type="radio" id="zToA" value="zToA" v-model="sortStyle" />
        <label for="zToA">Subject: Z to A</label>
      </p>
      <p>
        <input type="radio" id="LaToZ" value="LaToZ" v-model="sortStyle" />
        <label for="LaToZ">Location: A to Z</label>
      </p>
      <p>
        <input type="radio" id="LzToA" value="LzToA" v-model="sortStyle" />
        <label for="LzToA">Location: Z to A</label>
      </p>
      <p>
        <input
          type="radio"
          id="spaceLowHigh"
          value="spaceLowHigh"
          v-model="sortStyle"
        />
        <label for="spaceLowHigh">Spaces: Low to High</label>
      </p>
      <p>
        <input
          type="radio"
          id="spaceHighLow"
          value="spaceHighLow"
          v-model="sortStyle"
        />
        <label for="spaceHighLow">Spaces: High to Low</label>
      </p>
    </div>

    <div
      v-for="lesson in sortedProducts"
      id="subject-div"
      class="lesson"
      :key="lesson.id"
    >
      <img
        class="lessonImage"
        v-bind:src="lesson.image"
        height="40"
        width="40"
      />
      <h2>Subject: {{ lesson.subject }}</h2>
      <p>Location: {{ lesson.location }}</p>
      <p>Price: AED {{ lesson.price }}</p>
      <p>Spaces available: {{ lesson.spaces }}</p>
      <button
        id="cart-btn"
        v-on:click="addToCart(lesson)"
        v-if="canAddToCart(lesson)"
      >
        <i class="fas fa-plus"></i> Add to cart
      </button>
      <button disabled v-else><i class="fas fa-plus"></i> Add to cart</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "LessonList",
  props: ["lessons"],
  data() {
    return {
      searchTerm: "",
    };
  },
  methods: {
    addToCart(lesson) {
    //   console.log("Added lesson", lesson.id);
      this.$emit("addLesson", lesson);
      this.cart.push(lesson);
    },
    canAddToCart(lesson) {
      return lesson.spaces > 0;
    },
  },
  computed: {
    sortedProducts() {
      function compare(a, b) {
        if (a.price > b.price) return 1;
        if (a.price < b.price) return -1;
        return 0;
      }
      return this.lessons.sort(compare);
    },
  },
};
</script>
