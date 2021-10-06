<script>
  import Modal from "../components/Modal.svelte";
  let db,
    posts = [],
    request = indexedDB.open("GratefulDiary", 3),
    showModal = false,
    currentEntryBody,
    currentEntryHeader,
    currentEntryTimestamp;
  request.onerror = function (event) {
    console.log("Why didn't you allow my web app to use IndexedDB?! 😭😿");
    console.error("Database error: " + event.target.errorCode);
  };
  request.onsuccess = function (event) {
    db = event.target.result;

    let transaction = db.transaction(["entries"], "readonly");
    let objectStore = transaction.objectStore("entries");
    let cursor = objectStore.openCursor();
    cursor.onsuccess = function (e) {
      let res = e.target.result;
      if (res) {
        posts.push(res.value);
        res.continue();
      }
    };
    transaction.oncomplete = function () {
      posts.forEach((post) => {
        post.html = post.body.replace(/^\t{3}/gm, "");
      });
      posts = posts;
    };
  };
  function selectEntry(entry) {
    currentEntryBody = entry.html;
    currentEntryHeader = entry.title;
    currentEntryTimestamp = new Date(entry.timestamp * 1000).toUTCString();
    showModal = true;
  }
  function typewriter(node, { speed = 50 }) {
    const valid =
      node.childNodes.length === 1 && node.childNodes[0].nodeType === 3;
    if (!valid) {
      throw new Error(
        `This transition only works on elements with a single text node child`
      );
    }
    const text = node.textContent;
    const duration = text.length * speed;
    return {
      duration,
      tick: (t) => {
        const i = ~~(text.length * t);
        node.textContent = text.slice(0, i);
      },
    };
  }
</script>

<svelte:head>
  <title>Grateful-ness 💀 Blog</title>
</svelte:head>

<h1>Recent entries</h1>

<ul>
  {#each posts as post}
    <!-- we're using the non-standard `rel=prefetch` attribute to
				tell Sapper to load the data for the page as soon as
				the user hovers over the link or taps it, instead of
				waiting for the 'click' event -->
    <li class="entry" on:click={() => selectEntry(post)}>
      {post.title} - {new Date(post.timestamp * 1000).toUTCString()}
    </li>
  {/each}
</ul>

{#if showModal}
  <Modal on:close={() => (showModal = false)}>
    <h2 slot="header">
      {currentEntryHeader}
    </h2>

    <div in:typewriter slot="body">
      {@html currentEntryBody}
    </div>

    <div slot="timestamp">
      {currentEntryTimestamp}
    </div>
  </Modal>
{/if}

<style>
  ul {
    margin: 0 0 1em 0;
    line-height: 1.5;
  }
  .entry {
    cursor: pointer;
  }
</style>
