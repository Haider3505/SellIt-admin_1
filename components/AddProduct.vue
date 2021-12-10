<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 class="header__title">Add a product</h2>
            <!-- <form action="" method="POST" enctype="multipart/form-data"> -->
            <form action="">
              <!-- category dropdown -->
              <div class="a-spacing-top-medium">
                <label for="">Category</label>
                <select class="a-select-option" v-model="categoryID">
                  <option
                    v-for="category in categories"
                    :value="category._id"
                    :key="category._id"
                  >
                    {{ category.type }}
                  </option>
                </select>
              </div>
              <!-- Owner Dropdown -->
              <div class="a-spacing-top-medium">
                <label for="">Owner</label>
                <select class="a-select-option" v-model="ownerID">
                  <option
                    v-for="owner in owners"
                    :value="owner._id"
                    :key="owner._id"
                  >
                    {{ owner.name }}
                  </option>
                </select>
              </div>

              <!-- Product Title -->

              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Title</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="title"
                />
              </div>
              <!-- Price -->
              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Price</label>
                <input
                  type="number"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="price"
                />
              </div>

              <!-- stockQuantity -->
              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Stock Qunatity</label>
                <input
                  type="number"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="stockQuantity"
                />
              </div>

              <!-- description -->

              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Description</label>
                <textarea
                  placeholder="Describe your product"
                  style="width: 100%"
                  v-model="description"
                />
              </div>

              <!-- photo -->

              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input
                      type="file"
                      @change="onFileSelected"
                      name="photo"
                      multiple
                    />
                    <p style="margin-top: -70px">{{ fileName }}</p>
                  </label>
                </div>
              </div>

              <hr />
              <!-- Submit Button -->
              <div class="a-spacing-top-large">
                <span class="a-button-register">
                  <span class="a-button-inner">
                    <span class="a-button-text" @click="submitData"
                      >Add Product</span
                    >
                  </span>
                </span>
              </div>
            </form>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>
<script>
export default {
  props: ["categories", "owners"],

  data() {
    return {
      categoryID: null,
      ownerID: null,
      title: "",
      price: 0,
      description: "",
      stockQuantity: 1,
      selectedFiles: null,
      fileName: "",
    };
  },
  methods: {
    onFileSelected(e) {
      this.selectedFiles = e.target.files[0];
      // this.selectedFiles = e.target.files;
      // console.log(this.selectedFiles);
      // this.fileName = "Files has been selected" + this.selectedFiles;
    },
    async submitData() {
      try {
        let data = new FormData();
        data.append("title", this.title);
        data.append("description", this.description);
        data.append("price", this.price);
        data.append("stockQuantity", this.stockQuantity);
        data.append("ownerID", this.ownerID);
        data.append("categoryID", this.categoryID);
        // data.append("photo[]", this.selectedFiles);
        data.append("photo", this.selectedFiles);

        //POST api

        let result = await this.$axios.$post(
          "http://localhost:3000/api/products",
          data
        );

        console.log(result);

        this.$router.push("/");
      } catch (error) {
        console.log(error);
      }
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
