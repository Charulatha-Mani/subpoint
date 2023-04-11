<template>
  <div>
    <link rel="stylesheet" href="style.css" />

    <!-- Creates fields for the user to type their details -->
    <h2>Checkout</h2>
    <form id="userInfo" class="cartStyle">
      <h2 style="font-size: 25px">Information</h2>
      <p>
        <input v-model="firstName" placeholder="First Name" />
      </p>
      <p>
        <input v-model="lastName" placeholder="Last Name" />
      </p>
      <p>
        <input v-model="phoneNo" placeholder="Phone" />
      </p>
      <!-- Button is disabled if any fields are empty or don't match the RegEx -->
      <button disabled v-if="emptyFields" id="cart-btn" class="placeOrdBtn">
        Place Order
      </button>
      <button
        @click.prevent="placeOrder()"
        v-else
        id="cart-btn"
        class="placeOrdBtn"
      >
        Place Order<lord-icon
          src="https://cdn.lordicon.com/lqsduwhb.json"
          trigger="hover"
          style="width: 40px; height: 40px"
        >
        </lord-icon>
      </button>
    </form>

    <!-- The field shows the user entered info and cart items -->
    <div id="cartCheckOut" class="cartStyle">
      <h2 style="font-size: 25px">Order Summary</h2>

      <p>First Name: {{ firstName }}</p>
      <p>Last Name: {{ lastName }}</p>
      <p>Phone: {{ phoneNo }}</p>

      <hr width="100%" style="border: 2px dotted purple" />

      <h2 class="pageHeader">Your Cart</h2>
      <!-- Loops through the products and if they match the ID in the cart it shows it on this page as well as their frequency -->
      <div v-for="lesson in lessons" class="cartProducts" :key="lesson.id">
        <span v-text="showInCart(lesson, lesson._id)"></span>
        <span v-if="counter(lesson._id) > 0"
          >&nbsp;&nbsp;&nbsp;&nbsp;X&nbsp;&nbsp;&nbsp;&nbsp;</span
        >
        <span v-text="counter(lesson._id)"></span>
        <span v-if="counter(lesson._id) > 0">&nbsp;&nbsp;&nbsp;&nbsp;AED</span>
        <span v-text="showInCartPrice(lesson, lesson._id)"></span>
        <!-- <p>{{cartItem.subject}}</p> -->
        <button
          v-text="inCartButton"
          v-if="counter(lesson._id) > 0"
          v-on:click="removeFromCart(lesson)"
          id="removeItmBtn"
        ></button>
      </div>
      <hr width="100%" style="border: 1px solid violet; background: violet" />
    </div>
  </div>
</template>

<script>
var nameRegExp = new RegExp("^[A-Za-z]{1,}$");
// Created RegEx for the phone number to start with '05' and have 9 other digits
var phoneRegExp = new RegExp("^05[0-9]{8,8}$");

export default {
  name: "Form",
  props: ["cart"],
  data() {
    return {
      firstName: "",
      lastName: "",
      phoneNo: "",
    };
  },
  methods: {
    removeFromCart(lesson) {
      this.$emit("removeLesson", lesson.id);
    },
    placeOrder(event) {
      
      alert("Order Submitted!");
      event.preventDefault();
      //   location.reload();
    },
    counter(_id) {
      let counter = 0;
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i] === _id) {
          counter++;
        }
      }
      if (counter > 0) {
        return counter;
      }
    },
    // If the product is in the cart at least once it will display it on the checkout page
    showInCart(lesson, _id) {
      let counter = this.counter(_id);
      if (counter > 0) {
        return lesson.subject;
      }
    },
    showInCartPrice(lesson, _id) {
      let counter = this.counter(_id);
      if (counter > 0) {
        return lesson.price;
      }
    },
  },
  computed: {
    emptyFields() {
      return (
        !nameRegExp.test(this.firstName) ||
        !nameRegExp.test(this.lastName) ||
        !phoneRegExp.test(this.phoneNo)
      );
    },
  },
};
</script>
