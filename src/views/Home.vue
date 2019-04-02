<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>MY name is {{ name }}!</p>
    <!-- <h1>New Recipe</h1>
    <button v-on:click="createProduct()">Create product</button>
    <div v-for="product in products">
      <h2>Title: {{ product.title }}</h2>
      <img v-bind:src="product.primary_image" v-bind:alt="product.name" />
      <p>Name: {{ product.name }}</p>
      <!-- <p>My products: {{ products }}</p> -->

    <h1>New Product</h1>
    <div>
      Name:
      <input type="text" v-model="newProductName" />
      Price:
      <input type="text" v-model="newProductPrice" />
      Description:
      <input type="text" v-model="newProductDescription" />
      Instock:
      <input type="text" v-model="newProductInstock" />
      Supplier ID:
      <input type="text" v-model="newProductSupplierId" />
      User ID:
      <input type="text" v-model="newUserId" />
    </div>
    <button v-on:click="createProduct()">Create product</button>
    <div v-for="product in products">
      <h2>Title: {{ product.title }}</h2>
      <img v-bind:src="product.primary_image" v-bind:alt="product.name" />
      <div>
        <button v-on:click="showProduct(product)">More info</button>
      </div>

      <div v-if="product === currentProduct">
        <p>Name: {{ product.name }}</p>
        <p>My products: {{ product }}</p>
        <p>Price: {{ product.price }}</p>
        <p>Description: {{ product.description }}</p>
        <div>
          <h4>Edit product</h4>
          Title:
          <input type="text" v-model="product.name" />
          Chef:
          <input type="text" v-model="product.price" />
          Prep time:
          <input type="text" v-model="product.description" />
          Ingredients:
          <input type="text" v-model="product.instock" />
          Directions:
          <input type="text" v-model="product.supplier_id" />
          <button v-on:click="updateProduct(product)">Update product</button>
          <button v-on:click="destroyProduct(product)">Destroy product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
var axios = require("axios");

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      name: "Peter",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductInstock: "",
      newProductSupplierId: "",
      newUserId: "",
      currentProduct: null
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(this.products);
    });
  },
  methods: {
    createProduct: function() {
      console.log("Create the Product");
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        instock: this.newProductInstock,
        supplier_id: this.newProductSupplierId,
        user_id: this.newUserId
      };
      axios.post("/api/products", params).then(response => {
        this.products.push(response.data);
      });
    },
    showProduct: function(product) {
      if (this.currentProduct === product) {
        this.currentProduct = null;
      } else {
        this.currentProduct = product;
      }
    },
    updateProduct: function(product) {
      var params = {
        name: product.name,
        price: product.price,
        description: product.description,
        instock: product.instock,
        supplier_id: product.supplier_id
      };
      axios.patch("/api/products/" + product.id, params).then(response => {
        console.log("Successfully updated", response.data);
        product = response.data;
      });
    },
    destroyProduct: function(product) {
      axios.delete("/api/products/" + product.id).then(response => {
        console.log("Successfully destroyed product", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>
