<template>
  <div class="product">
    <div v-if="isLoading && !isError">
      <h1>Loading...</h1>
    </div>
    <div v-else-if="isError">
      <h1>Something went wrong</h1>
      <button @click="fetchProducts">Try Again</button>
    </div>
    <div v-else :class="pageClass">
      <span class="background">&nbsp;</span>
      <div v-if="pageClass === 'unavailable-page'" class="unavailable-card">
        <img
          src="../assets/image/sad-face.png"
          alt="sad-face"
          class="sad-face"
        />
        <p>This product is unavailable to show</p>
        <button @click="nextProduct">Next</button>
      </div>
      <div v-else class="card">
        <img :src="product.image" alt="product image" class="product-img" />
        <div class="detail">
          <h1 class="title">{{ product.title }}</h1>
          <div class="subtitle">
            <p class="category">{{ product.category }}</p>
            <span class="rating">
              <p class="num-rating">{{ product.rating.rate }}/5</p>
              <div class="star-rating">
                <div
                  v-for="n in 5"
                  :key="n"
                  class="star"
                  :class="{ filled: n <= Math.round(product.rating.rate) }"
                ></div>
              </div>
            </span>
          </div>
          <div class="desc">
            <p>{{ product.description }}</p>
          </div>
          <p class="price">${{ product.price }}</p>
          <div class="buttons">
            <button class="buy">Buy Now</button>
            <button @click="nextProduct">Next</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductPage",
  data() {
    return {
      product: {},
      isLoading: true,
      isError: false,
      index: 0,
    };
  },
  computed: {
    id() {
      return this.index + 1;
    },
    pageClass() {
      console.log(this.product.category);
      return (
        this.product &&
        (this.product.category === "men's clothing"
          ? "men-page"
          : this.product.category === "women's clothing"
          ? "women-page"
          : "unavailable-page")
      );
    },
  },
  methods: {
    async fetchProduct() {
      const URL = `https://fakestoreapi.com/products/${this.id}`;
      try {
        const response = await fetch(URL);
        const data = await response.json();
        console.log(data);
        this.product = data;
        this.isLoading = false;
        this.isError = false;
      } catch (error) {
        console.log(error);
        this.isError = true;
      }
    },
    nextProduct() {
      this.index = (this.index + 1) % 20;
      this.isLoading = true;
      this.fetchProduct();
    },
  },
  mounted() {
    this.fetchProduct();
  },
};
</script>

<style scoped src="../assets/style/page.css"></style>
