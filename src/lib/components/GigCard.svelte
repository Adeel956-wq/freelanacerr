<script>
	let { gig } = $props();
</script>

<article class="group flex flex-col overflow-hidden rounded-2xl border border-slate-200/80 bg-white transition-all duration-200 hover:-translate-y-1 hover:shadow-xl">
	<!-- Cover Image -->
	<div class="relative aspect-[16/10] overflow-hidden bg-slate-100">
		<img
			src={gig.image}
			alt={gig.title}
			class="h-full w-full object-cover transition-transform duration-300 group-hover:scale-105"
		/>
		{#if gig.badge}
			<span class="absolute top-3 left-3 rounded-md bg-slate-900/80 backdrop-blur-md px-2.5 py-1 text-xs font-semibold text-white" >
				{gig.badge}
			</span>
		{/if}
	</div>

	<!-- Content Body -->
	<div class="flex flex-1 flex-col p-5">
		<!-- Seller Profile -->
		<div class="flex items-center gap-3">
			<img src={gig.avatar} alt={gig.sellerName} class="h-8 w-8 rounded-full object-cover" />
			<div>
				<h4 class="text-xs font-bold text-slate-800">{gig.sellerName}</h4>
				<p class="text-[10px] font-medium text-blue-600">{gig.sellerLevel}</p>
			</div>
		</div>

		<!-- Title -->
		<h3 class="mt-3 line-clamp-2 text-sm font-semibold text-slate-800 group-hover:text-blue-600">
			{gig.title}
		</h3>

		<!-- Rating -->
		<div class="mt-3 flex items-center gap-1.5 text-xs">
			<span class="text-amber-400">★</span>
			<span class="font-bold text-slate-800">{gig.rating}</span>
			<span class="text-slate-400">({gig.reviewsCount})</span>
		</div>

		<!-- Pricing Footer -->
		<div class="mt-auto pt-4 border-t border-slate-100 flex items-center justify-between">
			<span class="text-xs text-slate-400">Starting at</span>
			<span class="text-lg font-black text-slate-900">${gig.price}</span>
		</div>
	</div>
</article>