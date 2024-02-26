<template>
  <div class="container" v-if="productData">
    <div class="product">
      <div class="product-picture">
        <img :src="productData.image" alt="" class="product-preview" />
      </div>
      <div class="product-detail">
        <h2
          class="product-name"
          :class="{
            'blue-text': isMensClothing,
            'purple-text': isWomensClothing,
          }"
        >
          {{ productData.title }}
        </h2>
        <div class="category-rating">
          <p class="category">{{ productData.category }}</p>
          <p class="rating">
            {{ productData.rating.rate + "/" + productData.rating.count }}
          </p>
        </div>
        <div class="description-container">
          <p class="description">{{ productData.description }}</p>
        </div>
        <p
          class="price"
          :class="{
            'blue-text': isMensClothing,
            'purple-text': isWomensClothing,
          }"
        >
          {{ "$" + productData.price }}
        </p>
        <div class="action-button">
          <button
            class="buy-button"
            :class="{
              'blue-buy-button': isMensClothing,
              'purple-buy-button': isWomensClothing,
            }"
          >
            Buy Now
          </button>
          <button
            class="next-button"
            :class="{
              'blue-next-button': isMensClothing,
              'purple-next-button': isWomensClothing,
            }"
            @click="getNextProduct"
          >
            Next Product
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
let currentIndex = 1;
export default {
  name: "ProductDisplay",
  data() {
    return {
      productData: null,
    };
  },
  computed: {
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
        const response = await fetch(
          `https://fakestoreapi.com/products/${currentIndex}`
        );
        this.productData = await response.json(); // Convert response to JSON
      } catch (error) {
        console.error("Error fetching product data:", error);
      }
      console.log(this.productData);
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
.container {
  background-color: #d6e6ff;
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.container .product {
  background-color: #ffffff;
  width: 70%;
  height: 80%;
  border-radius: 10px;
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.container .product .product-picture {
  width: 100%;
  height: 100%;
  display: grid;
  align-items: center;
  justify-items: center;
}

.container .product .product-picture .product-preview {
  width: 80%;
  height: 80%;
}

.container .product .product-detail {
  width: 90%;
  height: 90%;
  display: flex;
  flex-direction: column;
  text-align: left;
}

.container .product .product-detail .category-rating {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
}

.container .product .product-detail .description-container {
  width: 100%;
  height: 40%;
  border-top: 1px solid #dcdcdc;
  border-bottom: 1px solid #dcdcdc;
}

.container .action-button {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.container .action-button .buy-button {
  width: 45%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
}

.container .action-button .next-button {
  background-color: #ffffff;
  width: 45%;
  height: 35px;
  border-radius: 5px;
  cursor: pointer;
}

.blue-text {
  color: #002772;
}

.purple-text {
  color: #720060;
}

.blue-buy-button {
  color: #ffffff;
  background-color: #002772;
}

.purple-buy-button {
  color: #ffffff;
  background-color: #720060;
}

.blue-next-button {
  color: #002772;
  border: 3px solid #002772;
}

.purple-next-button {
  color: #720060;
  border: 3px solid #720060;
}
</style>
