<main class="container">
	<h1>MPS ID Lookup</h1>

	<div class="center">
		<input type="text" bind:value={searchTerm} placeholder="ID" class="search-bar">
	</div>

	{#if searchTerm}
		<h2><b>Photo of student with ID: {searchTerm}</b></h2>

		{#await searchImages(searchTerm)}
			<p>Loading...</p>
		{:then results}
			{#if results.length === 0}
				<p>There is no photo for MPS student ID {searchTerm}</p>
			{:else}
				<ul class="image-list">
					{#each results as image}
						<li>
							<img src={image} alt="Photo of student with ID: {searchTerm}">
						</li>
					{/each}
				</ul>
			{/if}
		{:catch error}
			<p>Error: {error.message}</p>
		{/await}
	{/if}


</main>

  
  
<style>
	.container {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  justify-content: center;
	  height: 50vh;
	}
  
	.search-bar {
	  position: sticky; /* Update the position to sticky */
	  top: 0;
	  z-index: 1;
	  padding: 0.5rem;
	  background-color: #fff;
	  border: 1px solid #ccc;
	}
  
	.image-list {
	  list-style-type: none;
	  padding: 0;
	  margin-top: 2rem;
	}
  </style>
  
  
  <script>
	import { createEventDispatcher } from 'svelte';
  
	const dispatch = createEventDispatcher();
	let searchTerm = '';
  
	function handleSearch() {
	  // Emit the search term to the parent component
	  dispatch('search', searchTerm);
	}
  
	// Function to search for images with the given term
	// @ts-ignore
	async function searchImages(term) {
	  try {
		const response = await fetch(`/photos/${term}.jpeg`);
		if (response.ok) {
		  // Return the image URL if found
		  return [`/photos/${term}.jpeg`];
		} else {
		  // Return an empty array if image not found
		  return [];
		}
	  } catch (error) {
		throw new Error('There is no photo for MPS student ID: {searchTerm}');
	  }
	}
  </script>
  