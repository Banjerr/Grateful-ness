<script>
  export let segment;
</script>

<nav>
  <ul>
    <li>
      <a class={segment === undefined ? "selected" : ""} href=".">home</a>
    </li>

    <li>
      <a class={segment === "blog" ? "selected" : ""} href="blog">blog</a>
    </li>

    <li>
      <a class={segment === "account" ? "selected" : ""} href="account">
        account
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
