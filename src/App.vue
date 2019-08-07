<template>
  <div class="container">
    <div class="jumbotron jumbotron-fluid">
      <div class="container">
        <h1 class="display-4">Cofundie Payments</h1>
        <p class="lead">Rent homes <i>courtesy meqasa</i></p>
      </div>
    </div>
    <div class="row">
      <div class="col-md-7">
        <div class="row">
          <div class="col-md-6" v-bind:key="product.id" v-for="product in products">
            <product
              v-on:add-to-cart="addToCart(product)"
              :product="product"
              :isInCart="isInCart(product)"
            ></product>
          </div>
        </div>
      </div>
      <div class="col-md-5 my-5">
        <cart v-on:remove-from-cart="removeFromCart($event)" v-on:pay="pay()" :items="cart"></cart>
      </div>
    </div>
  </div>
</template>

<script>
import products from "@/products.json";
import Cart from "@/components/Cart";
import Product from "@/components/Product";
export default {
  name: "app",
  components: {
    Product,
    Cart
  },
  methods: {
    addToCart(product) {
      this.cart.push(product);
    },
    isInCart(product) {
      const item = this.cart.find(p => p.id === product.id);
      return !!item;
    },
    removeFromCart(product) {
      this.cart = this.cart.filter(item => item.id !== product.id);
    },
    pay() {
      this.cart = [];
      // alert("Your shopping is finished!!");
    }
  },
  data() {
    return { products, cart: [] };
  }
};
</script>

<style>
body {
  background-color: #fbf8f3;
}
</style>