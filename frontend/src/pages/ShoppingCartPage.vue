<template>
  <h1>Shopping Cart</h1>
  <div v-if="cartItems.length">
    <ShoppingCartList @remove-from-cart="removeFromCart($event)" :products="cartItems" />
    <button class="checkout-button">Proceed to Checkout</button>
  </div>
  <div v-else>
    <p>Your cart is empty!</p>
  </div>
</template>

<script>
import axios from "axios";
import ShoppingCartList from "@/components/ShoppingCartList";

export default {
  name: "ShoppingCartPage",
  props: ["user"],
  components: {
    ShoppingCartList,
  },
  data() {
    return {
      cartItems: [],
    };
  },
  watch: {
    async user(newUserValue) {
      if (newUserValue) {
        const cartResponse = await axios.get(`/api/users/${newUserValue.uid}/cart`);
        this.cartItems = cartResponse.data;
      }
    }
  },
  methods: {
    async removeFromCart(productId){
      const response = await axios.delete(`/api/users/${this.user.uid}/cart/${productId}`);
      this.cartItems = response.data;
    },
  },
  async created() {
    if (this.user) {
      const response = await axios.get(`/api/users/${this.user.uid}/cart`);
      this.cartItems = response.data;
    }
  }
};
</script>
