<template>
  <!--
        element will only be rendered based on category (v-if="isMensClothing",v-if="isWomensClothing",v-if="!isProductAvailable) when it's true, and it will be hidden when it's false
      -->

  <!--Loading...-->
  <div class="loader-container" v-if="loading">
    <div class="loading-spinner"></div>
  </div>

  <div class="container" v-else-if="!loading">
    <!--v-if="productData-->
    <section class="men-section" v-if="isMensClothing">
      <div class="bg-container">
        <img
          src="../assets/powder-blue-web-solid-color-background.jpg"
          alt=""
          class="bg-img"
        />
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
            <p class="rating">
              {{ productData.rating.rate + "/" + 5 }}
            </p>
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
        <img
          src="../assets/queen-pink-solid-color-background.jpg"
          alt=""
          class="bg-img"
        />
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
            <p class="rating">
              {{ productData.rating.rate + "/" + 5 }}
            </p>
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
        <img
          src="../assets/light-gray-solid-color-background.jpg"
          alt=""
          class="bg-img"
        />
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
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

.loader-container {
  width: 100%;
  height: 100vh;
  display: grid;
  align-content: center;
  justify-content: center;
}

.loading-spinner {
  border: 16px solid #f3f3f3;
  border-radius: 50%;
  border-top: 16px solid #3498db;
  width: 120px;
  height: 120px;
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
}

/* Safari */
@-webkit-keyframes spin {
  0% {
    -webkit-transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
  }
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.container {
  background-color: #ffffff;
  color: #1e1e1e;
  font-family: "Poppins", sans-serif;
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container .bg-container {
  width: 100%;
  height: 71%;
  position: absolute;
  top: 0;
  left: 0;
}

.container .bg-img {
  width: 100%;
  height: 100%;
}

.container .product {
  background-color: #ffffff;
  width: 70%;
  height: 80%;
  border-radius: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding: 10px;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.5);
  z-index: 1;
}

.container .men-section,
.container .women-section,
.container .unavailable-section {
  background-color: #ffffff;
  color: #1e1e1e;
  font-family: "Poppins", sans-serif;
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container .product .product-picture {
  width: 100%;
  height: 100%;
  display: grid;
  align-items: center;
  justify-items: center;
}

.container .product .product-picture .product-preview {
  width: 300px;
  height: 400px;
}

.container .product .product-detail {
  width: 95%;
  height: 95%;
  display: flex;
  flex-direction: column;
  text-align: left;
  padding: 5px;
}

.container .product .product-detail .product-name-men {
  height: 100px;
  color: #002772;
}

.container .product .product-detail .product-name-women {
  height: 100px;
  color: #720060;
}

.container .product .product-detail .category-rating {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  margin-bottom: -15px;
  font-size: 14px;
  font-weight: 400;
}

.container .product .product-detail .category-rating .category {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

.container .product .product-detail .description-container {
  width: 100%;
  height: 200px;
  border-top: 1px solid #dcdcdc;
  border-bottom: 1px solid #dcdcdc;
  overflow: scroll;
  padding: 5px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  font-size: 16px;
  font-weight: 400;
}

.container .product .product-detail .price-men {
  margin: 8px 0;
  font-weight: 800;
  color: #002772;
}

.container .product .product-detail .price-women {
  margin: 8px 0;
  font-weight: 800;
  color: #720060;
}

.container .action-button {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.container .action-button .buy-button-men {
  width: 45%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 800;
  color: #ffffff;
  background-color: #002772;
}
.container .action-button .buy-button-women {
  width: 45%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 800;
  color: #ffffff;
  background-color: #720060;
}

.container .action-button .next-button-men {
  background-color: #ffffff;
  width: 45%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 800;
  color: #002772;
  border: 3px solid #002772;
}

.container .action-button .next-button-women {
  background-color: #ffffff;
  width: 45%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 800;
  color: #720060;
  border: 3px solid #720060;
}

.container .unavailable-section .unavailable-page {
  background-color: #ffffff;
  width: 70%;
  height: 80%;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: "Poppins", sans-serif;
  font-weight: 400;
  padding: 10px;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.5);
  background-image: url(../assets/../assets/sad-512-mod-s.png);
  background-size: cover;
  background-position: center;
  z-index: 1;
}

.container .unavailable-page .unavailable-message {
  color: #1e1e1e;
}

.container .unavailable-page .next-button1 {
  background-color: #ffffff;
  border: 3px solid #1e1e1e;
  color: #3f3f3f;
  width: 50%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 800;
}

@media only screen and (max-width: 600px) {
  .container .bg-container {
    height: 250px;
  }

  .container .product {
    grid-template-columns: none;
    width: 85%;
    height: 300px;
    position: absolute;
    top: 5px;
  }

  .container .product .product-picture {
    width: 100%;
    height: 100%;
  }

  .container .product .product-picture .product-preview {
    width: 250px;
    height: 300px;
  }

  .container .product .product-detail .product-name-men,
  .container .product .product-detail .product-name-women {
    font-size: 20px;
    font-weight: 400;
    height: 50px;
  }

  .container .product .product-detail .description-container {
    height: 100px;
    font-size: 16px;
  }
}
</style>
