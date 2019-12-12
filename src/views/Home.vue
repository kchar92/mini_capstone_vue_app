<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <p>Product name <input type="text" v-model="productName"></p>
    <p>Product description <input type="text" v-model="productDescription"></p>
    <p>Product price <input type="text" v-model="productPrice"></p>

    <button v-on:click="createProduct()">Create Product</button>

    <div v-for="product in products">
      <p> {{ product.name }}</p>
      <p> {{ product.description }}</p>
      <p> {{ product.image_url }} </p>
      <img v-bind:src="product.image_url">

      <button v-on:click="toggleInfo(product)">See more</button>

      <div v-if="currentProduct === product">
        <p>price: {{ product.price }}</p>

        <p>name: <input type="text" v-model="product.name"></p>
        <p>description: <input type="text" v-model="product.description"></p>
        <p>price: <input type="text" v-model="product.price"></p>
        <button v-on:click="updateProduct(product)">Update</button>

      </div>

      <hr>
    </div>

    
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      productName: "",
      productDescription: "",
      productPrice: "",
      currentProduct: {}
    };
  },
  created: function() {
    console.log('created here');
    axios.get('/api/products').then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    createProduct: function() {
      console.log('creating product');

      var params = {
        name: this.productName,
        description: this.productDescription,
        price: this.productPrice
      };

      axios.post('/api/products', params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
        this.productName = "";
        this.productDescription = "";
        this.productPrice = "";
      });
    },
    toggleInfo: function(theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      console.log('toggling info');
    },
    updateProduct: function(theProduct) {
      console.log("updating product");
      console.log(theProduct);

      var params = {
        name: theProduct.name,
        description: theProduct.description,
        price: theProduct.price
      };

      axios.patch(`/api/products/${theProduct.id}`, params).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.description = response.data.description;
        theProduct.price = response.data.price;
      });
    }
  }
};
</script>
