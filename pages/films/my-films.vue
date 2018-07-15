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
      </div>
    </div>
    <hr>
    <div class="container">
      <h1 class="title">List Of My Films</h1>
      <div class="columns is-desktop is-multiline">
        <div class="column is-one-quarter" v-for="(item, index) in films" :key="index">
          <div class="card">
            <div class="card-image">
              <figure class="image is-4by3">
                <a :href="`/films/${item.slug}`"><img :src="`${baseImgUrl}/storage/images/${item.photo}`"></a>
              </figure>
            </div>
            <div class="card-content">
              <div class="media">
                <div class="media-left">
                  <figure class="image is-48x48">
                    <img src="https://bulma.io/images/placeholders/96x96.png" alt="Avatar image">
                  </figure>
                </div>
                <div class="media-content">
                  <p class="title is-4">{{ item.title }}</p>
                  <p class="subtitle is-6">@johnsmith</p>
                </div>
              </div>

              <div class="content">
                {{ item.description }}
                <a :href="`/films/${item.slug}`">read more</a>
                <br>
                <p><span class="tag is-info">Rating: {{ item.rating }}</span><span class="tag is-info">Price: {{ item.price }}</span><span class="tag is-info">Country: {{ item.country }}</span><span class="tag is-info">Genre: {{ item.genre }}</span></p>
                <br>
                Released: <time datetime="`${item.release_on}`">{{ item.release_on }}</time>
              </div>
            </div>
          </div>
        </div>
      </div>
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
      baseImgUrl: process.env.baseImgUrl,
      headers: { Authorization: `Bearer ${this.$store.state.authUser}` }
    };
  },
  async asyncData({ store, env }) {
    store.commit("SET_HEAD", ["Film gallery", "Manage your films here"]);
    let response = await fetch(`${env.baseUrl}/films/mine`, {
      method: "POST",
      headers: { Authorization: `Bearer ${store.state.authUser}` }
    });

    return {
      films: await response.json(),
      headers: { Authorization: `Bearer ${store.state.authUser}` }
    };
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

p > span {
  margin-right: 10px;
  margin-bottom: 10px;
}
</style>
