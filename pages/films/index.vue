<template>
  <section class="container">
    <div class="container">
      <a href="/"><app-logo/></a>
      <h1 class="title">
        Laravel Film
      </h1>
      <div class="container links">
        <nuxt-link to="/films" class="button--green">Films</nuxt-link>
        <nuxt-link to="/films/create" class="button--green">Create film</nuxt-link>
      </div>
    </div>
    <hr>
    <div class="container">
      <h2 class="list-gallery">List Of Films</h2>
      <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
        <thead>
          <tr>
            <th><abbr title="ID">ID</abbr></th>
            <th>Photo</th>
            <th><abbr title="Title">Title</abbr></th>
            <th><abbr title="Description">Description</abbr></th>
            <th><abbr title="Created">Created</abbr></th>
            <th><abbr title="Updated">Updated</abbr></th>
            <th>Actions</th>
          </tr>
        </thead>
        <!-- <tfoot>
          <tr>
            <th><abbr title="Position">Pos</abbr></th>
            <th>Team</th>
            <th><abbr title="Played">Pld</abbr></th>
            <th><abbr title="Won">W</abbr></th>
            <th><abbr title="Drawn">D</abbr></th>
            <th><abbr title="Lost">L</abbr></th>
            <th><abbr title="Goals for">GF</abbr></th>
            <th><abbr title="Goals against">GA</abbr></th>
            <th><abbr title="Goal difference">GD</abbr></th>
            <th><abbr title="Points">Pts</abbr></th>
            <th>Qualification or relegation</th>
          </tr>
        </tfoot> -->
        <tbody>
          <tr v-for="(item, index) in films_mine" :key="index">
            <th>{{ item.id }}</th>
            <td>
              <figure class="image is-128x128">
                <a :href="`/films/${item.slug}`">
                  <img :src="`${baseImgUrl}/storage/images/${item.photo}`" width="128" height="128" style="clear: both; max-height: 100px;">
                </a>
              </figure>
            </td>
            <td>{{ item.title }}</td>
            <td>{{ item.description }}</td>
            <td>{{ item.created_at }}</td>
            <td>{{ item.updated_at }}</td>
            <td>
              <a :href="`/films/${item.slug}`" class="button is-small">Detail</a>
              <!-- <a href="javascript:;" class="button is-small is-danger">Delete</a> -->
            </td>
          </tr>
        </tbody>
      </table>
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
    let response = await fetch(`${env.baseUrl}/films`, {
      method: "GET",
      headers: { Authorization: `Bearer ${store.state.authUser}` }
    });

    return {
      films_mine: await response.json(),
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
</style>
