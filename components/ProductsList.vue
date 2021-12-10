<template>
  <main>
    <div class="a-spacing-large"></div>

    <!-- <div class="a-spacing-large"></div> -->
    <div class="container-fluid browsimg-history">
      <h2 class="header__title">Display All products</h2>
      <div class="row">
        <div
          v-for="(product, index) in products"
          :key="product._id"
          class="col-xl-2 col-lg-2 col-md-3 col-sm-6 col-6 br bb"
        >
          <div class="history-box">
            <!-- product image -->
            <div class="img-container">
              <a href="#" class="a-link-normal">
                <img
                  :src="'http://localhost:3000/' + product.photo"
                  class="img-fluid"
                />
              </a>
            </div>
            <!-- Product Title -->
            <div class="a-spacing-top-base asin-title">
              <span class="a-text-normal">
                <div class="pl3n-sc-truncated">{{ product.title }}</div>
              </span>
            </div>
            <!-- Product Ratings -->
            <div class="a-row">
              <a href="#">
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-star"></i>
              </a>
              <span class="a-letter-space"></span>
              <span class="a-color-tertiary a-size-small a-asin-reviews"
                >(1732)</span
              >
            </div>

            <!-- product price -->
            <div class="a-row">
              <span class="a-size-base a-color-price">
                <span class="pl3n-sc-price">Pkr:{{ product.price }}</span>
              </span>
            </div>

            <!-- product buttons -->

            <div class="a-row">
              <button
                @click="$emit('productIdEmitUpdate', product._id)"
                class="a-button-history margin-right-10"
              >
                Update
              </button>
              <!-- DELETE -->
              <button
                class="a-button-history margin-right-10"
                @click="$emit('productIdEmitDelete', product._id, index)"
              >
                Delete
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  props: ["products"],
  emits: ["productIdEmitUpdate", "productIdEmitDelete"],
};
</script>

<style scoped>
.img-container {
  min-height: 150px;
  max-height: 200px;
}
.img {
  max-height: 200px;
}
main {
  width: 98%;
}
.header__title {
  text-align: center;
  background-color: darkslategrey;
  color: white;
  width: 100%;
}
</style>
