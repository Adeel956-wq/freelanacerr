<script lang="ts">
	import { createEventDispatcher } from 'svelte';

	interface Category {
		id: string;
		name: string;
		icon: string;
		count: number;
	}

	let { categories = [], activeCategory = 'all' } = $props();

	const dispatch = createEventDispatcher();

	function selectCategory(id: string) {
		dispatch('selectCategory', id);
	}
</script>

<section class="mx-auto max-w-7xl px-4 py-16 sm:px-6 lg:px-8">
	<div class="flex items-end justify-between mb-8">
		<div>
			<h2 class="text-2xl font-bold tracking-tight text-slate-900 sm:text-3xl">Browse Categories</h2>
			<p class="mt-1 text-slate-500">Explore talent across high-demand specialization areas.</p>
		</div>
		{#if activeCategory !== 'all'}
			<button
				onclick={() => selectCategory('all')}
				class="text-sm font-semibold text-blue-600 hover:underline cursor-pointer"
			>
				Reset Filter
			</button>
		{/if}
	</div>

	<div class="grid grid-cols-2 gap-4 sm:grid-cols-3 lg:grid-cols-4">
		{#each categories as cat}
			<button
				onclick={() => selectCategory(cat.id)}
				class="group text-left rounded-2xl border p-5 transition-all duration-200 hover:-translate-y-1 hover:shadow-xl {activeCategory === cat.id ? 'border-blue-600 bg-blue-50/50 ring-2 ring-blue-600/20' : 'border-slate-200/80 bg-white hover:border-blue-200'}"
			>
				<span class="inline-block rounded-xl bg-slate-100 p-3 text-2xl transition-transform group-hover:scale-110">
					{cat.icon}
				</span>
				<h3 class="mt-4 font-bold text-slate-800">{cat.name}</h3>
				<p class="mt-1 text-xs text-slate-500">{cat.count}</p>
			</button>
		{/each}
	</div>
</section>