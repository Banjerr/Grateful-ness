<script>
  import * as fcl from "@onflow/fcl";
  import { onMount } from "svelte";
  import { stores } from "@sapper/app";
  import { userDataStore } from "../userData";

  const { session } = stores();

  const { ENV, ACCESS_NODE, WALLET_DISCOVERY } = $session;

  let parsedUserData = null;

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
      userDataStore.update(() => parsedUserData);
      console.log("userData", parsedUserData);
    }
  });

  function logoutUser() {
    console.log("logging out");
    fcl.unauthenticate();
    userDataStore.set(null);
  }

  function loginUser() {
    console.log("logging in");
    fcl.authenticate();
    userDataStore.update(() => parsedUserData);
  }
</script>

<button on:click={$userDataStore !== null ? logoutUser : loginUser}>
  {$userDataStore !== null ? "Logout" : "Login"}
</button>
<br />
<pre>
  <code>
    {$userDataStore !== null
      ? JSON.stringify($userDataStore, null, 2)
      : "No user data"}
  </code>
</pre>
