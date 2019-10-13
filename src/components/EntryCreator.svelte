<script>
	let db;
    let request = indexedDB.open("GratefulDiary", 2);
    let currentEntryBody,
        currentEntryTitle;
    const currentEntryTimestamp = Math.floor(Date.now() / 1000);

	request.onerror = function(event) {
		console.log("Why didn't you allow my web app to use IndexedDB?! 😭😿");
		console.error("Database error: " + event.target.errorCode);
	};

	request.onsuccess = function(event) {
		db = event.target.result;
	}

	request.onupgradeneeded = function (event) {
        let objectStore;
        db = event.target.result;
        
        if(!db.objectStoreNames.contains("entries")) {
            console.log('gonna make it')
            objectStore = db.createObjectStore("entries", { keyPath: "timestamp" });
        } 

		objectStore.createIndex("title", "title", { unique: false });

		objectStore.createIndex("timestamp", "timestamp", { unique: true });

		objectStore.createIndex("body", "body", { unique: false });
    };
    
    function handleNewEntry() {
        //Get a transaction
        //default for OS list is all, default for type is read
        const transaction = db.transaction(["entries"],"readwrite");
        //Ask for the objectStore
        const store = transaction.objectStore("entries");

        //Define the entry
        const entry = {
            title: currentEntryTitle,
            body: currentEntryBody,
            timestamp: currentEntryTimestamp
        }

        //Perform the add
        const request = store.add(entry);

        request.onerror = function(e) {
            console.log("Error",e.target.error.name);
            //some type of error handler
        }

        request.onsuccess = function(e) {
            console.log("Woot! Did it");
        }
    }
</script>

<input type="text" name="entry-title" id="entry-title" bind:value={currentEntryTitle} placeholder='What are you Grateful for today?' />
<textarea name="entry-body" id="entry-body" bind:value={currentEntryBody} placeholder="Today I'm Grateful because..."></textarea>
<button on:click={handleNewEntry}></button>
