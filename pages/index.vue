<template>
  <div class="row">
    <div class="col-sm-2 bg-dark height" style="height: 100vh">
      <p class="pt-5 pb-5 text-center">
        <a class="text-decoration-none"
          ><img style="width: 100%" src="https://i.imgur.com/OH9ESfy.png"
        /></a>
      </p>
      <hr class="bg-light" />
      <p class="pt-2 pb-2 text-center">
        <nuxt-link to="/ownerForm" class="text-decoration-none"
          ><span class="text-light">Profile</span></nuxt-link
        >
      </p>
      <hr class="bg-light" />
      <p
        class="pt-2 pb-2 text-center"
        @click="renderComponent('products-list')"
      >
        <a class="text-decoration-none"
          ><span class="text-light">View Products</span></a
        >
      </p>

      <hr class="bg-light" />
      <p class="pt-2 pb-2 text-center" @click="renderComponent('add-product')">
        <a class="text-decoration-none"
          ><span class="text-light">Add Products</span></a
        >
      </p>
      <hr class="bg-light" />
      <p class="pt-2 pb-2 text-center" @click="renderComponent('add-category')">
        <a class="text-decoration-none"
          ><span class="text-light">Add Categories</span></a
        >
      </p>
      <hr class="bg-light" />

      <p class="pt-2 pb-2 text-center" @click="renderComponent('add-owner')">
        <a class="text-decoration-none"
          ><span class="text-light">Add Store</span></a
        >
      </p>
      <hr class="bg-light" />
      <!-- <hr class="bg-light" />
      <p class="pt-2 pb-2 text-center">
        <a href="display-orders.php" class="text-decoration-none"
          ><span class="text-light">View Orders</span></a
        >
      </p> -->
    </div>
    <!-- hii -->

    <div class="col-sm-10 bg-light">
      <div class="row">
        <!-- test -->
        <div class="wrapper" v-if="currentTabComponent === ''">
          <div class="typing-demo">Welcome to the Admin Panel of Sell_IT</div>
        </div>
        <!-- test -->
        <component
          v-else
          :is="currentTabComponent"
          :product="product"
          :products="products"
          :owners="owners"
          :categories="categories"
          @productIdEmitUpdate="updateProduct"
          @productIdEmitDelete="deleteProduct"
          class="tab"
        ></component>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  // asyncData (Nuxt feature) : fetch the data before the nuxt app finished loading (Good for SEO)
  async asyncData({ $axios, params }) {
    try {
      let productsApi = $axios.$get("http://localhost:3000/api/products");
      let categoriesApi = $axios.$get("http://localhost:3000/api/categories");
      let ownersApi = $axios.$get("http://localhost:3000/api/owners");
      // let productApi = $axios.$get(
      //   `http://localhost:3000/api/products/${params.id}`
      // );
      // calling these apis in parallel

      const [catResponse, ownerRespone, productsResponse] = await Promise.all([
        categoriesApi,
        ownersApi,
        productsApi,
      ]);

      return {
        categories: catResponse.categories,
        owners: ownerRespone.owners,
        products: productsResponse.products,
      };
    } catch (err) {
      console.log(err);
    }
  },
  data() {
    return {
      product: "",
      currentTabComponent: "",
    };
  },
  methods: {
    renderComponent(componentName) {
      this.currentTabComponent = componentName;
      console.log(this.currentTabComponent);
    },

    async deleteProduct(id, index) {
      try {
        let response = await this.$axios.delete(
          `http://localhost:3000/api/products/${id}`
        );

        if (response.status) {
          this.products.splice(index, 1);
          this.$toast
            .success("product has been deleted successfully")
            .goAway(4000);
        }

        // console.log(response);
      } catch (err) {
        console.log(err);
      }
    },
    async updateProduct(id) {
      try {
        let response = await this.$axios.get(
          `http://localhost:3000/api/products/${id}`
        );
        this.product = response.data.product;
        return {
          product: response.data.product,
        };

        // console.log(response);
      } catch (err) {
        console.log(err);
      } finally {
        this.renderComponent("UpdateProduct");
      }
    },
    // ADD PRODUCT"S METHODS
  },
};
</script>
<style scoped>
.header__title {
  text-align: center;
  background: color #4f4f4f;
  width: 100%;
}
p {
  cursor: pointer;
}
.row {
  margin-right: 0px !important;
  margin-left: 0px !important;
}

.text-light:hover {
  color: orange !important;
}
.wrapper {
  height: 100vh;
  /*This part is important for centering*/
  display: flex;
  align-items: center;
  justify-content: center;
}

.typing-demo {
  width: 46ch;
  padding-bottom: 80px;
  padding-left: 150px;
  animation: typing 5s steps(30), blink 0.5s step-end infinite alternate;
  white-space: nowrap;
  overflow: hidden;
  border-right: 3px solid;
  font-family: monospace;
  font-size: 2em;
}

@keyframes typing {
  from {
    width: 0;
  }
}

@keyframes blink {
  50% {
    border-color: transparent;
  }
  100% {
    border-color: gray;
  }
}
</style>
