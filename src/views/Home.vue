<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <p> {{ product.name }}</p>
      <p> {{ product.description }}</p>
      <p> {{ product.image_url }} </p>
      <img v-bind:src="product.image_url">
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
      products: []
    };
  },
  created: function() {
    console.log('created here');
    axios.get('/api/products').then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {}
};
</script>
