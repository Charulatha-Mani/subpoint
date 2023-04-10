<template>
  <div id="app">
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css"
    />
    <link rel="stylesheet" href="style.css" />
    <header>
      <img src="gradcap.png" height="40" width="40" />
      <h1 class="heading">SUBPOINT</h1>
      <!-- Created cart button that activates only if there's something in the cart -->
      <button
        v-on:click="showCheckout"
        v-if="cartCount && showProduct"
        id="cartBtn"
      >
        <i class="fa-solid fa-basket-shopping fa-3x"></i>
        <span class="badge">{{ cartCount }}</span>
      </button>
      <!-- This was made in case the cart was emptied and otherwise you could not go back to product page -->
      <button id="cartBtn" v-on:click="showCheckout" v-else-if="!showProduct">
        <i class="fa-solid fa-basket-shopping fa-3x"></i>
        <span class="badge">{{ cartCount }}</span>
      </button>
    </header>

    <div v-if="showProduct">
      <div>
        <lesson-list :lessons="lessons" @addLesson="addToCart"></lesson-list>
      </div>
    </div>

    <div class="checkout" v-else>
      <button v-on:click="showCheckout()" id="carttobackbtn">
        <span class="fas fa-arrow-left"> Back </span>
      </button>

      <checkout :cart="cart" @removeLesson="removeFromCart"></checkout>
    </div>

    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  </div>
</template>

<script>
import lessonList from "./components/LessonList.vue";
import checkout from "./components/CheckoutForm.vue";


export default {
  name: "App",
  components: {
    lessonList,
    checkout,
  },
  data() {
    return {
      sitename: "SUBPOINT",
      cart: [],
      lessons: [],
      showProduct: true,
    };
  },
  // data:  {

  //     cartButton: '<i class="fas fa-shopping-cart"></i> Checkout',
  // },
  mounted() {
    fetch("http://localhost:3000/collection/lessons").then((response) => {
      response.json().then((json) => {
        this.lessons = json;
        console.log(this.lessons);
        console.log(json);
      });
    });
  },
  methods: {
    addToCart(lesson) {
      this.cart.push(lesson);
      lesson.spaces--;
    },
    removeFromCart(lesson) {
      const index = this.cart.indexOf(lesson.id);
      if (index > -1) {
        this.cart.splice(index, 1);
      }
      lesson.spaces++;
    },
    showCheckout() {
      this.showProduct = this.showProduct ? false : true;
    }
  },
  // computed: {
  //   cartCount() {
  //     return this.cart.length || ''
  //   },
  // }
};
</script>
