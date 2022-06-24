<script lang="ts">
  import Router from "svelte-spa-router"
  import PersonalPage from "./personalPage/PersonalPage.svelte"
  import APIUser from "./routes/APIUser.svelte"


  let routes = {
    "/": PersonalPage,
    "/APIUser": APIUser,
  }

</script>

<main>
  <Router {routes} />
</main>

<style>

</style>
