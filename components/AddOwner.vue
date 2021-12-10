<template>
  <main>
    <div class="container-fluid">
      <div class="row">
        <div class="col-sm-3"></div>
        <div class="col-sm-6">
          <div class="a-section">
            <div class="a-spacing-top-medium"></div>
            <h2 class="header__title">Add a Store</h2>
            <form action="">
              <!-- owner name -->

              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Name</label>
                <input
                  type="text"
                  class="a-input-text"
                  style="width: 100%"
                  v-model="name"
                />
              </div>

              <!-- about input -->

              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">About</label>
                <textarea
                  placeholder="owner's about info"
                  style="width: 100%"
                  v-model="about"
                />
              </div>

              <!-- photo -->

              <div class="a-spacing-medium">
                <label style="margin-bottom: 0px">Add Photo</label>
                <div class="a-row a-spacing-top-medium">
                  <label class="choosefile-button">
                    <i class="fal fa-plus"></i>
                    <input type="file" @change="onFileSelected" />
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
                      >Add Owner</span
                    >
                  </span>
                </span>
              </div>
            </form>

            <br />

            <ul class="list-group-item">
              <li v-for="owner in owners" :key="owner._id">{{ owner.name }}</li>
            </ul>
          </div>
        </div>
        <div class="col-sm-3"></div>
      </div>
    </div>
  </main>
</template>
<script>
export default {
  props: ["owners"],
  data() {
    return {
      name: "",
      about: "",
      selectedFile: null,
      fileName: "",
    };
  },
  methods: {
    onFileSelected(e) {
      this.selectedFile = e.target.files[0];

      this.fileName = e.target.files[0].name;
    },
    async submitData() {
      let data = new FormData();
      data.append("name", this.name);
      data.append("about", this.about);
      data.append("photo", this.selectedFile);

      //POST api
      try {
        let result = await this.$axios.$post(
          "http://localhost:3000/api/owners",
          data
        );
        console.log(result);
        this.owners.push(data);
      } catch (error) {
        console.log(error);
      }

      //   this.$router.push("/");
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

ul {
  border: 2px solid red;
  margin: 0%;
  padding: 0;
}
li {
  border-bottom: 1px solid rgba(200, 0, 0, 0.5);
  padding: 12px;
  text-align: center;
}
li:last-child {
  border-bottom: none;
}
.header__title {
  text-align: center;
  background-color: darkslategrey;
  color: white;
  width: 100%;
}
main {
  width: 90%;
}
</style>
