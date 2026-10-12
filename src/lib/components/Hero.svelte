<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	let query = $state('');

	function onSearch(e) {
		e.preventDefault();
		dispatch('search', { query });
	}
</script>

<section class="relative overflow-hidden bg-slate-900 py-20 text-white lg:py-28">
	<!-- Background Gradient Accent -->
	<div class="pointer-events-none absolute -top-40 -right-40 h-96 w-96 rounded-full bg-blue-600/30 blur-3xl"></div>
	<div class="pointer-events-none absolute -bottom-40 -left-40 h-96 w-96 rounded-full bg-indigo-600/20 blur-3xl"></div>

	<div class="relative mx-auto max-w-5xl px-4 text-center sm:px-6 lg:px-8">
		<!-- Pill Tag -->
		<span class="inline-flex items-center gap-2 rounded-full border border-blue-500/30 bg-blue-500/10 px-4 py-1.5 text-xs font-semibold text-blue-400">
			⚡ Trusted by 10,000+ Businesses Worldwide
		</span>

		<h1 class="mt-6 text-4xl font-black tracking-tight text-white sm:text-6xl lg:text-7xl">
			Scale Your Business With Expert Freelancers
		</h1>
		<p class="mx-auto mt-6 max-w-2xl text-lg text-slate-300">
			Find top-rated talent for software engineering, design, marketing, and AI development in minutes.
		</p>

		<!-- Search Bar -->
		<form onsubmit={onSearch} class="mx-auto mt-10 max-w-2xl">
			<div class="flex flex-col gap-2 rounded-2xl bg-white p-2 sm:flex-row sm:items-center shadow-2xl">
				<div class="flex flex-1 items-center px-3">
					<svg class="h-5 w-5 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
						<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
					</svg>
					<input
						type="text"
						bind:value={query}
						placeholder="Try 'SvelteKit Developer' or 'UI Design'..."
						class="w-full bg-transparent px-3 py-2 text-slate-800 placeholder-slate-400 focus:outline-none"
					/>
				</div>
				<button
					type="submit"
					class="w-full cursor-pointer rounded-xl bg-blue-600 px-7 py-3.5 font-bold text-white transition hover:bg-blue-700 sm:w-auto"
				>
					Search
				</button>
			</div>
		</form>

		<!-- Quick Tags -->
		<div class="mt-6 flex flex-wrap items-center justify-center gap-2 text-xs text-slate-400">
			<span class="font-semibold text-slate-300">Popular:</span>
			<button onclick={() => { query = 'React'; onSearch(); }} class="rounded-full cursor-pointer border border-slate-700 px-3 py-1 hover:border-slate-500 hover:text-white transition">React</button>
			<button onclick={() => { query = 'UI/UX Design'; onSearch(); }} class="rounded-full cursor-pointer border border-slate-700 px-3 py-1 hover:border-slate-500 hover:text-white transition">UI/UX Design</button>
			<button onclick={() => { query = 'AI App'; onSearch(); }} class="rounded-full  cursor-pointer border border-slate-700 px-3 py-1 hover:border-slate-500 hover:text-white transition">AI App</button>
			<button onclick={() => { query = 'SEO'; onSearch(); }} class="rounded-full cursor-pointer  border border-slate-700 px-3 py-1 hover:border-slate-500 hover:text-white transition">SEO</button>
		</div>
	</div>
</section>