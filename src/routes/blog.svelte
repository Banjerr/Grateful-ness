<script>
	let db,
		posts = [],
		request = indexedDB.open("GratefulDiary", 3);

	request.onerror = function(event) {
		console.log("Why didn't you allow my web app to use IndexedDB?! 😭😿");
		console.error("Database error: " + event.target.errorCode);
	}

	request.onsuccess = function(event) {
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
		}

		transaction.oncomplete = function() {
			posts.forEach(post => {
				post.html = post.body.replace(/^\t{3}/gm, '');
			});

			posts = posts;
		}
	}
</script>

<style>
	ul {
		margin: 0 0 1em 0;
		line-height: 1.5;
	}
</style>

<svelte:head>
	<title>Blog</title>
</svelte:head>

<h1>Recent entries</h1>

<ul>
	{#each posts as post}
		<!-- we're using the non-standard `rel=prefetch` attribute to
				tell Sapper to load the data for the page as soon as
				the user hovers over the link or taps it, instead of
				waiting for the 'click' event -->
		<li>{post.title} - {new Date(post.timestamp * 1000).toUTCString()}</li>
	{/each}
</ul>