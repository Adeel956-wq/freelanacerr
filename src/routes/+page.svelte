<script>
	import Navbar from '$lib/components/Navbar.svelte';
	import Hero from '$lib/components/Hero.svelte';
	import CategoryGrid from '$lib/components/CategoryGrid.svelte';
	import GigCard from '$lib/components/GigCard.svelte';
	import Footer from '$lib/components/Footer.svelte';
	import { categories, featuredGigs } from '$lib/data/mockData.js';

	let activeCategory  = $state('all');
	let searchQuery = '';

	const filteredGigs = $derived(featuredGigs.filter((gig) => {
		const matchesCategory = activeCategory === 'all' || gig.category === activeCategory;
		const matchesSearch =
			gig.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
			gig.sellerName.toLowerCase().includes(searchQuery.toLowerCase());
		return matchesCategory && matchesSearch;
	}));

	function handleCategorySelect(event) {
		activeCategory = event.detail;
	}

	function handleSearch(event ) {
		searchQuery = event.detail.query;
	}
</script>

<div class="min-h-screen bg-slate-50 font-sans text-slate-900 antialiased">
	<Navbar />
	
	<Hero on:search={handleSearch} />

	<CategoryGrid
		{categories}
		{activeCategory}
		on:selectCategory={handleCategorySelect}
	/>

	<!-- Featured Services Section -->
	<section class="mx-auto max-w-7xl px-4 py-8 sm:px-6 lg:px-8">
		<div class="flex items-center justify-between mb-8">
			<div>
				<h2 class="text-2xl font-bold tracking-tight text-slate-900 sm:text-3xl">
					Top Performing Gigs
				</h2>
				<p class="mt-1 text-slate-500">Hand-picked services with stellar client satisfaction.</p>
			</div>
		</div>

		{#if filteredGigs.length === 0}
			<div class="rounded-2xl border border-dashed border-slate-300 bg-white p-12 text-center">
				<p class="text-slate-500">No services found matching your criteria.</p>
				<button
					onclick={() => { activeCategory = 'all'; searchQuery = ''; }}
					class="mt-4 font-semibold text-blue-600 hover:underline"
				>
					Clear Filters
				</button>
			</div>
		{:else}
			<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-4">
				{#each filteredGigs as gig (gig.id)}
					<GigCard {gig} />
				{/each}
			</div>
		{/if}
	</section>

	<Footer />
</div>