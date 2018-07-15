<template>
  <section class="container">
    <div class="container">
      <a href="/"><app-logo/></a>
      <h1 class="title">
        Laravel Film
      </h1>
      <div class="container links">
        <nuxt-link to="/films" class="button--green">All films</nuxt-link>
        <nuxt-link to="/films/my-films" class="button--green">My films</nuxt-link>
        <nuxt-link to="/films/create" class="button--green">Create film</nuxt-link>
        <h2 class="subtitle">Add Film</h2>
      </div>
    </div>
    <hr>
    <div class="container">
      <form id="film-create" enctype="multipart/form-data" method="post">
        <div class="field">
          <p class="control has-icons-left has-icons-right">
            <input v-validate="'required'" v-model="title" :class="{'input': true, 'is-danger': errors.has('title') }" type="text" name="title" placeholder="Film name">
            <span class="icon is-small is-left">
              <i class="fas fa-title"/>
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check"/>
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check"/>
              <i v-show="errors.has('title')" class="fa fa-warning"/>
            </span>
            <span v-show="errors.has('title')" class="help is-danger">{{ errors.first('title') }}</span>
          </p>
        </div>

        <div class="field">
          <p class="control has-icons-left has-icons-right">
            <input v-validate="'required'" v-model="description" :class="{'input': true, 'is-danger': errors.has('description') }" type="text" name="description" placeholder="Description">
            <span class="icon is-small is-left">
              <i class="fas fa-text"/>
            </span>
            <span class="icon is-small is-right">
              <i class="fas fa-check"/>
              <i v-show="errors.has('description')" class="fa fa-warning"/>
            </span>
            <span v-show="errors.has('description')" class="help is-danger">{{ errors.first('description') }}</span>
          </p>
        </div>

        <div class="field">
          <p class="control has-icons-left">
            <input v-validate="'required'" v-model="release_on" :class="{'input': true, 'is-danger': errors.has('release_on') }" type="text" name="release_on" placeholder="Released on">
            <span class="icon is-small is-left">
              <i class="fas fa-lock"/>
            </span>
            <span class="icon is-small is-right">
              <i v-show="errors.has('release_on')" class="fa fa-warning"/>
            </span>
            <span v-show="errors.has('release_on')" class="help is-danger">{{ errors.first('release_on') }}</span>
          </p>
        </div>

        <div class="field">
          <p class="control has-icons-left">
            <input v-validate="'required'" v-model="rating" :class="{'input': true, 'is-danger': errors.has('rating') }" type="number" min="1" max="5" name="rating" placeholder="Rating 1-5">
            <span class="icon is-small is-left">
              <i class="fas fa-lock"/>
            </span>
            <span class="icon is-small">
              <i v-show="errors.has('rating')" class="fas fa-warning"/>
            </span>
            <span v-show="errors.has('rating')" class="help is-danger">{{ errors.first('rating') }}</span>
          </p>
        </div>

        <div class="field">
          <p class="control has-icons-left">
            <input v-validate="'required'" v-model="price" :class="{'input': true, 'is-danger': errors.has('price') }" type="number" min="1" max="1000" name="price" placeholder="Price">
            <span class="icon is-small is-left">
              <i class="fas fa-lock"/>
            </span>
            <span class="icon is-small">
              <i v-show="errors.has('price')" class="fas fa-warning"/>
            </span>
            <span v-show="errors.has('price')" class="help is-danger">{{ errors.first('price') }}</span>
          </p>
        </div>

        <div class="field">
          <p class="control has-icons-left">
            <input v-validate="'required'" v-model="country" :class="{'input': true, 'is-danger': errors.has('country') }" type="text" name="country" placeholder="Country">
            <span class="icon is-small is-left">
              <i class="fas fa-lock"/>
            </span>
            <span class="icon is-small">
              <i v-show="errors.has('country')" class="fas fa-warning"/>
            </span>
            <span v-show="errors.has('country')" class="help is-danger">{{ errors.first('country') }}</span>
          </p>
        </div>

        <div class="field">
          <p class="control has-icons-left">
            <input v-validate="'required'" v-model="genre" :class="{'input': true, 'is-danger': errors.has('genre') }" type="text" name="genre" placeholder="Genre">
            <span class="icon is-small is-left">
              <i class="fas fa-lock"/>
            </span>
            <span class="icon is-small">
              <i v-show="errors.has('genre')" class="fas fa-warning"/>
            </span>
            <span v-show="errors.has('genre')" class="help is-danger">{{ errors.first('genre') }}</span>
          </p>
        </div>

        <div class="field">
          <label>Upload photo for this film</label>
          <input v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('photo') }" name="photo" type="file" placeholder="Choose image" @change="onFileChange">
          <!-- <span v-show="errors.has('photo')" class="help is-danger">{{ errors.first('photo') }}</span> -->
        </div>

        <div class="field">
          <p class="control">
            <button type="button" class="button is-success" @click="addFilm()">
              Save
            </button>
          </p>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
import AppLogo from "~/components/AppLogo.vue";

export default {
  components: {
    AppLogo
  },
  middleware: "auth",
  data() {
    return {
      formErrors: [],
      title: "",
      description: "",
      release_on: "",
      rating: "",
      price: "",
      country: "",
      genre: "",
      photo: "",
      headers: { Authorization: `Bearer ${this.$store.state.authUser}` }
    };
  },
  methods: {
    onFileChange(e) {
      let files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.createImage(files[0]);
      // console.log(files[0])
    },
    createImage(file) {
      // console.log(file);
      let reader = new FileReader();
      let vm = this;
      reader.onload = e => {
        vm.photo = e.target.result;
      };
      reader.readAsDataURL(file);
    },
    addFilm() {
      this.$nuxt.$loading.start();
      var formData = new FormData();
      // console.log(this.photo)
      // Fields in the post
      formData.append("photo", this.photo);
      formData.append("title", this.title);
      formData.append("description", this.description);
      formData.append("release_on", this.release_on);
      formData.append("rating", this.rating);
      formData.append("price", this.price);
      formData.append("country", this.country);
      formData.append("genre", this.genre);
      const url = `${process.env.baseUrl}/films/create`;
      fetch(url, {
        method: "POST",
        headers: {
          Authorization: `Bearer ${this.$store.state.authUser}`
        },
        body: formData
      })
        .then(response => response.json())
        .then(responseJson => {
          this.formErrors = [];
          if (responseJson.errors) {
            this.formErrors = responseJson.errors;
            // console.log(this.formErrors.length)
          }
          if (responseJson.success) {
            this.$router.push("/films");
          }
        })
        .catch(error => {
          console.log("formError: " + error);
          // this.postSubmit(['Oops, something Went Wrong.'])
        });

      this.$nuxt.$loading.finish();
    }
  }
};
</script>

<style>
/*.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}*/

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: bold;
  font-size: 20px;
  color: #35495e;
  text-transform: uppercase;
}

.subtitle {
  font-weight: 300;
  font-size: 20px;
  color: #526488;
  word-spacing: 5px;
  padding-top: 15px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.form-group {
  text-align: left;
}

.form-group input {
  width: 100%;
  padding: 5px;
  margin: 10px 0;
}

/*.list {
  width: 100%;
}*/

.list {
  padding: 0;
  margin: 0;
  list-style: none;
  -ms-box-orient: horizontal;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -moz-flex;
  display: -webkit-flex;
  display: flex;
}

.row {
  -webkit-flex-direction: row;
  flex-direction: row;
}
</style>
