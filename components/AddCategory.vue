<template>
  <main>
    <div class="container-fluid c-section">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-spacing-top-medium"></div>
          <h2 style="text-align: center" class="header__title">
            Add a Category
          </h2>
          <form>
            <!-- category dropdown -->
            <div class="a-spacing-top-medium">
              <label for="">Type</label>
              <input class="a-input-text" style="width: 100%" v-model="type" />
            </div>

            <hr />
            <!-- Submit Button -->
            <div class="a-spacing-top-large">
              <span class="a-button-register" style="margin-left: 140px">
                <span class="a-button-inner">
                  <span class="a-button-text" @click="submitData"
                    >Add Category</span
                  >
                </span>
              </span>
            </div>
          </form>

          <br />

          <ul class="list-group-item">
            <li v-for="category in categories" :key="category._id">
              {{ category.type }}
              <hr />
            </li>
          </ul>
        </div>
      </div>
      <div class="col-sm-3"></div>
    </div>
  </main>
</template>
<script>
export default {
  props: ["categories"],

  data() {
    return {
      type: "",
    };
  },
  methods: {
    async submitData() {
      let data = { type: this.type };
      //POST api
      try {
        let result = await this.$axios.$post(
          "http://localhost:3000/api/categories",
          data
        );

        this.categories.push(data);
      } catch (error) {
        console.log(error);
      }

      //   this.$router.push("/");
    },
  },
};
</script>
<style scoped>
/* .img-fluid {
  height: 150px;
} */
main {
  width: 100%;
}
.header__title {
  text-align: center;
  background-color: darkslategrey;
  color: white;
  width: 100%;
}
</style>
