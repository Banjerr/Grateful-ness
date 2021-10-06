<script>
  let db;
  let request = indexedDB.open("GratefulDiary", 3);
  let currentEntryBody,
    currentEntryTitle,
    allPosts = [];
  const currentEntryTimestamp = Math.floor(Date.now() / 1000);
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
        allPosts.push(res.value);
        res.continue();
      }
    };
  };
  request.onupgradeneeded = function (event) {
    let objectStore;
    db = event.target.result;

    if (!db.objectStoreNames.contains("entries")) {
      objectStore = db.createObjectStore("entries", { keyPath: "timestamp" });
    }
    objectStore.createIndex("title", "title", { unique: false });
    objectStore.createIndex("timestamp", "timestamp", { unique: true });

    objectStore.createIndex("slug", "slug", { unique: true });
    objectStore.createIndex("body", "body", { unique: false });
  };
  function generateEntrySlug(string) {
    const a =
      "àáâäæãåāăąçćčđďèéêëēėęěğǵḧîïíīįìłḿñńǹňôöòóœøōõṕŕřßśšşșťțûüùúūǘůűųẃẍÿýžźż·/_,:;";
    const b =
      "aaaaaaaaaacccddeeeeeeeegghiiiiiilmnnnnooooooooprrsssssttuuuuuuuuuwxyyzzz------";
    const p = new RegExp(a.split("").join("|"), "g");
    return string
      .toString()
      .toLowerCase()
      .replace(/\s+/g, "-") // Replace spaces with -
      .replace(p, (c) => b.charAt(a.indexOf(c))) // Replace special characters
      .replace(/&/g, "-and-") // Replace & with 'and'
      .replace(/[^\w\-]+/g, "") // Remove all non-word characters
      .replace(/\-\-+/g, "-") // Replace multiple - with single -
      .replace(/^-+/, "") // Trim - from start of text
      .replace(/-+$/, ""); // Trim - from end of text
  }

  function handleNewEntry() {
    const transaction = db.transaction(["entries"], "readwrite");
    const store = transaction.objectStore("entries");
    const entry = {
      title: currentEntryTitle,
      body: currentEntryBody,
      timestamp: currentEntryTimestamp,
      slug: generateEntrySlug(currentEntryTitle),
    };
    const request = store.add(entry);
    request.onerror = function (e) {
      console.log("Error", e.target.error.name);
      //some type of error handler
      clearEntry();
    };
    request.onsuccess = function (e) {
      console.log("Woot! Did it");
      // some type of success handler
      clearEntry();
    };
  }
  function clearEntry() {
    currentEntryBody = null;
    currentEntryTitle = null;
  }
</script>

<div class="form-container">
  <input
    type="text"
    name="entry-title"
    id="entry-title"
    bind:value={currentEntryTitle}
    placeholder="What are you Grateful for today?"
  />
  <textarea
    name="entry-body"
    id="entry-body"
    bind:value={currentEntryBody}
    placeholder="Today I'm Grateful because..."
  />
  <button on:click={handleNewEntry}>Save</button>
  <button on:click={clearEntry}>Clear</button>
</div>

<style>
  input,
  textarea {
    width: 100%;
    margin: 0 auto 2%;
  }
  .form-container {
    text-align: right;
  }
</style>
