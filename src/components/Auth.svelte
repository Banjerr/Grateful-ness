<script>
  import * as fcl from "@onflow/fcl";
  import { onDestroy } from "svelte";
  import { stores } from "@sapper/app";
  import { userDataStore } from "../stores/userData";

  // get env vars
  const { session } = stores();
  const { ENV, ACCESS_NODE, WALLET_DISCOVERY } = $session;
  fcl
    .config({
      "fcl.appDomainTag": "Grateful-Ness",
    })
    .put("env", ENV)
    .put("accessNode.api", ACCESS_NODE)
    .put("discovery.wallet", WALLET_DISCOVERY)
    .put("app.detail.title", "Grateful-Ness");

  // get user data
  const unsubscribe = fcl.currentUser().subscribe((currentUser) => {
    if (currentUser && currentUser.loggedIn) {
      userDataStore.update(() => currentUser);
    } else {
      userDataStore.set(null);
    }
  });

  function logoutUser() {
    fcl.unauthenticate();
    userDataStore.set(null);
  }

  function loginUser() {
    fcl
      .authenticate()
      .then((currentUser) => {
        userDataStore.update(() => currentUser);
      })
      .catch((error) => {
        console.error(error);
      });
  }

  // clean up currentUser subscription
  onDestroy(() => {
    unsubscribe();
  });
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
