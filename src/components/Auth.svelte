<script>
  import * as fcl from "@onflow/fcl";
  import { onMount } from "svelte";
  import { stores } from "@sapper/app";

  const { session } = stores();

  const { ENV, ACCESS_NODE, WALLET_DISCOVERY } = $session;

  let userData = null,
    parsedUserData = null;

  fcl
    .config()
    .put("env", ENV)
    .put("accessNode.api", ACCESS_NODE)
    .put("discovery.wallet", WALLET_DISCOVERY)
    .put("app.detail.title", "Greateful-Ness");

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

  function logoutUser() {
    console.log("logging out");
    fcl.unauthenticate();
  }

  function loginUser() {
    console.log("logging in");
    fcl.authenticate();
  }
</script>

<button on:click={userData !== null ? logoutUser : loginUser}>
  {userData !== null ? "Logout" : "Login"}
</button>
