<script>
	import Header from "./components/Header.svelte";
	import CardLayout from "./components/CardLayout.svelte";
	import CardItem from "./components/CardItem.svelte";
	import Overlay from "./components/Overlay.svelte";
	import paginate from './components/paginate';
	import Pagination from './components/Pagination.svelte'

  let items = [];
  let currentPage = 1;
	let perPage = 1;
  let pageSize = 6;
	let loading = true;

	(async () => {
		await fetch("https://jsonplaceholder.typicode.com/photos?_limit=40").then(res => res.json()).then(
			data => {
				items = data;
				loading = false;
			}
		);
	})();

  $: paginatedItems = paginate({ items, pageSize, currentPage })

</script>

<main>
	<Header />
	{#if loading}
    <Overlay />
  {/if}
	<div class="container">
		<CardLayout>
			{#await paginatedItems}
				<Overlay />
			{:then items}
				{#each items as item}
					<CardItem {item} />
				{/each}
			{/await}
		</CardLayout>	
		
		<!-- Pagination -->
		<div class="pagination">
			<!-- Shows numbers -->
			{#if items.length > perPage}
				<Pagination
					totalItems="{items.length}"
					pageSize="{pageSize}"
					currentPage="{currentPage}"
					on:setPage="{(e) => currentPage = e.detail}"
				>
					<span slot="prev">«</span>
					<span slot="next">»</span>
				</Pagination>
			{/if}
		</div>
	</div>
</main>