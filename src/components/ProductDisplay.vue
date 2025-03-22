<template>
  <!--Loading...-->
  <div class="loader-container" v-if="loading">
    <div class="loading-spinner"></div>
  </div>

  <div class="container" v-else-if="!loading">
    <!--v-if="productData-->
    <section class="men-section" v-if="isMensClothing">
      <div class="bg-container">
        <img src="../assets/men-blue-background.jpg" alt="" class="bg-img" />
      </div>
      <div class="product">
        <div class="product-picture">
          <img :src="productData.image" alt="" class="product-preview" />
        </div>
        <div class="product-detail">
          <h2 class="product-name-men">
            {{ productData.title }}
          </h2>
          <div class="category-rating">
            <p class="category">
              {{ productData.category }}
            </p>
            <div class="rating">
              <p class="rating-number">
                {{ productData.rating.rate + "/" + 5 }}
              </p>
              <div
                class="circles-men"
                :style="{ '--rating': productData.rating.rate }"
              ></div>
            </div>
          </div>
          <div class="description-container">
            <p class="description">
              {{ productData.description }}
            </p>
          </div>
          <p class="price-men">
            {{ "$" + productData.price }}
          </p>
          <div class="action-button">
            <button class="buy-button-men">Buy Now</button>
            <button class="next-button-men" @click="getNextProduct">
              Next Product
            </button>
          </div>
        </div>
      </div>
    </section>
    <section class="women-section" v-if="isWomensClothing">
      <div class="bg-container">
        <img src="../assets/women-pink-background.jpg" alt="" class="bg-img" />
      </div>
      <div class="product">
        <div class="product-picture">
          <img :src="productData.image" alt="" class="product-preview" />
        </div>
        <div class="product-detail">
          <h2 class="product-name-women">
            {{ productData.title }}
          </h2>
          <div class="category-rating">
            <p class="category">
              {{ productData.category }}
            </p>
            <div class="rating">
              <p class="rating-number">
                {{ productData.rating.rate + "/" + 5 }}
              </p>
              <div
                class="circles-women"
                :style="{ '--rating': productData.rating.rate }"
              ></div>
            </div>
          </div>
          <div class="description-container">
            <p class="description">
              {{ productData.description }}
            </p>
          </div>
          <p class="price-women">
            {{ "$" + productData.price }}
          </p>
          <div class="action-button">
            <button class="buy-button-women">Buy Now</button>
            <button class="next-button-women" @click="getNextProduct">
              Next Product
            </button>
          </div>
        </div>
      </div>
    </section>
    <section class="unavailable-section" v-if="!isProductAvailable">
      <div class="bg-container">
        <img src="../assets/gray-background.jpg" alt="" class="bg-img" />
      </div>
      <div class="unavailable-page">
        <p class="unavailable-message">This product is unavailable to show</p>
        <button class="next-button1" @click="getNextProduct">
          Next Product
        </button>
      </div>
    </section>
  </div>
</template>

<script>
let currentIndex = 1;
export default {
  name: "ProductDisplay",
  data() {
    return {
      productData: null,
      loading: false, // Add loading state
    };
  },
  computed: {
    isProductAvailable() {
      return this.isMensClothing || this.isWomensClothing;
    },

    isMensClothing() {
      return this.productData && this.productData.category === "men's clothing";
    },

    isWomensClothing() {
      return (
        this.productData && this.productData.category === "women's clothing"
      );
    },
  },
  mounted() {
    this.fetchProductData();
  },

  methods: {
    async fetchProductData() {
      try {
        this.loading = true; // Set loading to true before making the API request

        const response = await fetch(
          `https://fakestoreapi.com/products/${currentIndex}`
        );
        this.productData = await response.json(); // Convert response to JSON
      } catch (error) {
        console.error("Error fetching product data:", error);
      } finally {
        this.loading = false; // Set loading to false after the request is complete
      }
      //console.log(this.productData);
    },
    getNextProduct() {
      // Increment index for the next product
      currentIndex = (currentIndex % 20) + 1;
      // Fetch and display the next product
      this.fetchProductData();
    },
  },
};
</script>

<style>
@import url("../assets/style/pages.css");
</style>
