<template>
  <section class="container">
    <div>
      <a href="/"><app-logo/></a>
      <h1 class="title">
        Laravel Film
      </h1>
      <h2 class="subtitle">
        Nuxt.js project
      </h2>
      <div class="links">
        <nuxt-link to="/auth/signin" class="button--grey">Signin</nuxt-link>
        <nuxt-link to="/auth/signup" class="button--grey">Signup</nuxt-link>
        <nuxt-link to="/films" class="button--green">All films</nuxt-link>
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
  middleware: "noauth"
};
</script>

<style>
.container {
  justify-content: center;
  align-items: center;
  text-align: center;
}

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
</style>
