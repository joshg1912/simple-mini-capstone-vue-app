<template>
  <div class="home">
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      Description:
      <input type="text" v-model="newProductParams.description" />
      Image_url:
      <input type="text" v-model="newProductParams.image_url" />
      Price:
      <input type="text" v-model="newProductParams.price" />
    </div>
    <button v-on:click="createProduct">Create a Product</button>
    <h1>{{ message }}</h1>
    <p>{{ greeting }}</p>

    <div v-for="product in products" v-bind:key="product.id">
      <h2>Name: {{ product.name }}</h2>
      <p>Price: {{ product.price }}</p>
      <img v-bind:src="product.image_url" :alt="product.name" />
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name:{{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Image_url: {{ currentProduct.image_url }}</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "This is Vue.js!",
      greeting: "here are products",
      products: [],
      newProductParams: {},
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All Products", this.products);
      });
    },
    createProduct: function () {
      console.log("create a product");
      axios.post("http://localhost:3000/products", this.newProductParams).then((response) => {
        console.log("Success!", response.data);
        this.products.push(response.data);
      });
    },
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
  },
};
</script>
