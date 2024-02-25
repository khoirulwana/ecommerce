<template>
  <div class="container" v-if="productData">
    <div class="product">
      <div class="product-picture">
        <img :src="productData.image" alt="" class="product-preview" />
      </div>
      <div class="product-detail">
        <h2 class="product-name">
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
        <p class="price">{{ "$" + productData.price }}</p>
        <div class="action-button">
          <button class="buy-button">Buy Now</button>
          <button class="next-button">Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      productData: null,
    };
  },
  mounted() {
    this.fetchProductData();
  },

  methods: {
    async fetchProductData() {
      try {
        const response = await fetch("https://fakestoreapi.com/products/1");
        this.productData = await response.json(); // Convert response to JSON
      } catch (error) {
        console.error("Error fetching product data:", error);
      }
      console.log(this.productData);
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
</style>
