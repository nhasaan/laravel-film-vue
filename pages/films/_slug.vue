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
      <div class="content">
        <h5>{{ item.title }} detail</h5>
        <hr class="is-clearfix">
        <figure>
          <img :src="`${baseImgUrl}/storage/images/${item.photo}`">
          <figcaption>
            Photo: {{ item.title }}
          </figcaption>
        </figure>
        <p><span class="tag is-info">Rating: {{ item.rating }}</span><span class="tag is-info">Price: {{ item.price }}</span><span class="tag is-info">Country: {{ item.country }}</span><span class="tag is-info">Genre: {{ item.genre }}</span></p>
        <h5>Description <span class="tag is-info">Last updated: {{ item.updated_at }}</span></h5>
        <p>{{ item.description }}</p>
        <h5>Comments</h5>
        <div class="box" v-for="(comment, index) in item.comments" :key="index">
          <article class="media">
            <div class="media-left">
              <figure class="image is-64x64">
                <img src="https://bulma.io/images/placeholders/128x128.png" alt="Image">
              </figure>
            </div>
            <div class="media-content">
              <div class="content">
                <p>
                  <strong>John Smith</strong> <small>@johnsmith</small> <small>31m</small>
                  <br>
                  {{ comment.comment }}
                </p>
              </div>
              <nav class="level is-mobile">
                <div class="level-left">
                  <a class="level-item" aria-label="reply">
                    <span class="icon is-small">
                      <i class="fas fa-reply" aria-hidden="true"></i>
                    </span>
                  </a>
                  <a class="level-item" aria-label="retweet">
                    <span class="icon is-small">
                      <i class="fas fa-retweet" aria-hidden="true"></i>
                    </span>
                  </a>
                  <a class="level-item" aria-label="like">
                    <span class="icon is-small">
                      <i class="fas fa-heart" aria-hidden="true"></i>
                    </span>
                  </a>
                </div>
              </nav>
            </div>
          </article>
        </div>
        <div class="">
          <div class="columns is-desktop">
            <div class="field">
              <p class="control has-icons-left has-icons-right">
                <input v-validate="'required'" v-model="comment" :class="{'input': true, 'is-danger': errors.has('comment') }" type="text" name="comment" placeholder="Comment">
                <span class="icon is-small is-left">
                  <i class="fas fa-title"/>
                </span>
                <span class="icon is-small is-right">
                  <i class="fas fa-check"/>
                </span>
                <span class="icon is-small is-right">
                  <i class="fas fa-check"/>
                  <i v-show="errors.has('comment')" class="fa fa-warning"/>
                </span>
                <span v-show="errors.has('comment')" class="help is-danger">{{ errors.first('comment') }}</span>
              </p>
            </div>

            <div class="field">
              <p class="control">
                <button type="button" class="button is-success" @click="addComment(item)">
                  Save
                </button>
              </p>
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
  data() {
    return {
      comment: "",
      baseImgUrl: process.env.baseImgUrl,
      headers: { Authorization: `Bearer ${this.$store.state.authUser}` }
    };
  },
  async asyncData({ store, params }) {
    // console.log("params: " + params.slug);
    // return false;
    store.commit("SET_HEAD", ["Film detail", "Film detail here"]);
    let response = await fetch(`${process.env.baseUrl}/films/${params.slug}`, {
      method: "GET",
      headers: { Authorization: `Bearer ${store.state.authUser}` }
    });

    return {
      item: await response.json(),
      headers: { Authorization: `Bearer ${store.state.authUser}` }
    };
  },
  methods: {
    async addComment(film) {
      this.$nuxt.$loading.start();
      const url = `${process.env.baseUrl}/films/${film.id}/comments`;
      await fetch(url, {
        method: "POST",
        headers: {
          Authorization: `Bearer ${this.$store.state.authUser}`,
          "Content-Type": "Application/json"
        },
        body: JSON.stringify({
          comment: this.comment
        })
      });

      // let comments = await fetch(
      //   `${process.env.baseUrl}/films/${params.slug}`,
      //   {
      //     method: "GET",
      //     headers: { Authorization: `Bearer ${this.$store.state.authUser}` }
      //   }
      // );
      // return {
      //   item: comments.json()
      // };

      this.$nuxt.$loading.finish();

      window.location.reload(true);
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

p > span {
  margin-right: 10px;
  margin-bottom: 10px;
}
</style>
