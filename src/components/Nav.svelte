<script>
  let userData = null,
    parsedUserData = null;
  import { onMount } from "svelte";
  onMount(() => {
    let tempUserdata = sessionStorage.getItem("CURRENT_USER");
    if (tempUserdata) {
      parsedUserData = JSON.parse(tempUserdata);
    }
    if (parsedUserData && parsedUserData.loggedIn) {
      userData = parsedUserData;
      console.log("userData", userData);
    }
  });
  export let segment;
</script>

<nav>
  <ul>
    <li>
      <a class={segment === undefined ? "selected" : ""} href=".">home</a>
    </li>

    <!-- for the blog link, we're using rel=prefetch so that Sapper prefetches the blog data when we hover over the link or tap it on a touchscreen -->
    <li>
      <a rel="prefetch" class={segment === "blog" ? "selected" : ""} href="blog"
        >blog</a
      >
    </li>

    <li>
      <a class={segment === "account" ? "selected" : ""} href="account">
        {userData ? "logout" : "login"}
      </a>
    </li>
  </ul>
</nav>

<style>
  nav {
    border-bottom: 1px solid rgba(255, 62, 0, 0.1);
    font-weight: 300;
    padding: 0 1em;
  }
  nav a {
    border-bottom: none;
  }
  ul {
    margin: 0;
    padding: 0;
  }
  /* clearfix */
  ul::after {
    content: "";
    display: block;
    clear: both;
  }
  li {
    display: block;
    float: left;
  }
  li:before {
    display: none;
  }
  a {
    text-decoration: none;
    padding: 1em 0.5em;
    display: block;
  }
</style>
