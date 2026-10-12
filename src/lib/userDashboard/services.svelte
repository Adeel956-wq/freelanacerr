<script lang="ts">
	// State for Modal control
	let isModalOpen = $state(false);

	// New Gig Form State

	let title = $state('');
	let category = $state('');
	let price = $state('');
	let description = $state('');
	let web_dev = $state('');
	let ui_ux = $state('');
	let digital_marketing  = $state('');
	let ai_dev = $state('');


	// fetch data
	async function services(e:any){
		e.preventDefault();
		let message = '';
		let loading = true;
		try{
			const response = await fetch("http://192.168.0.172:8080/services",{
				method:"POST",
				headers:{'content-Type':'application/json',},
				body:JSON.stringify({title,category,price,description})
			});
			if(response.ok){message="new services added successfully ";}
		}
		//else{message:"error no 091";}
		catch(err){console.log("error no sd12");err}
		finally{loading=true;}
	}
//services();
async function loadData(){
//e.preventDefault();
let message ="";let loading = true;
try{
	const load = await fetch("http://192.168.0.172:8080/getservices",{
		method:"POST",
		headers:{"content-Type":"application/json"}
	});
const result = await load.json();
data.services = result; // data ko update karna
}
catch(err_loading){message="error no fd12";console.error("error no fd12",err_loading)}
finally{loading=false}
}
loadData()

interface Service {
        title: string;
        category: string;
        price: number;
        description: string;
    }

    // data ko interface assign karein taake 'never' ka error khatam ho sake
     let data = $state<{ services: Service[] }>({ services: [] });

	
	// Sample Gigs List State
	
	// let gigs = $state([
	// 	{
	// 		id: 1,
	// 		title: 'Full-stack SvelteKit & Go Web Application Development',
	// 		category: 'Web Development',
	// 		price: 250,
	// 		impressions: 1240,
	// 		orders: 12,
	// 		rating: 4.9,
	// 		status: 'Active',
	// 		image: 'https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=300'
	// 	}
	// ]);

	// // Function to handle form submission
	// function handleCreateService(event: Event) {
	// 	event.preventDefault();

	// 	const newGig = {
	// 		id: Date.now(),
	// 		title,
	// 		category,
	// 		price: Number(price),
	// 		impressions: 0,
	// 		orders: 0,
	// 		rating: 0.0,
	// 		status: 'Active',
	// 		image: 'https://images.unsplash.com/photo-1581291518633-83b4ebd1d83e?w=300'
	// 	};

	// 	// Add new gig to list
	// 	gigs.unshift(newGig);

		// Form reset and modal close
		title = '';
		price = '';
		description = '';
		isModalOpen = false;
	
</script>

<div class="space-y-6">
	<!-- Top Bar -->
	<div class="flex items-center justify-between">
		<div>
			<h1 class="text-2xl font-black text-slate-900">My Gigs & Services</h1>
			<p class="text-sm text-slate-600">Manage your active listings and add new services.</p>
		</div>

		<!-- Open Modal Button -->
		<button
			onclick={() => (isModalOpen = true)}
			class="inline-flex items-center gap-2 rounded-xl bg-blue-600 px-5 py-2.5 text-sm font-bold text-white shadow-md transition hover:bg-blue-700 active:scale-95"
		>
			<svg class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" />
			</svg>
			Add New Service
		</button>
	</div>

	<!-- Gigs Cards List -->
	 <!-- <div class="grid grid-cols-1 gap-6 md:grid-cols-2 lg:grid-cols-3">
		{#each gigs as gig (gig.id)} 
			<div class="overflow-hidden rounded-2xl border border-slate-200 bg-white shadow-sm">
				<img src={gig.image} alt={gig.title} class="h-40 w-full object-cover" />
				<div class="p-4">
					<span class="text-xs font-semibold text-blue-600 uppercase">{gig.category}</span>
					<h3 class="mt-1 font-bold text-slate-800 line-clamp-2">{gig.title}</h3>
					<div class="mt-4 flex items-center justify-between border-t border-slate-100 pt-3">
						<span class="text-lg font-black text-slate-900">${gig.price}</span>
						<span class="rounded-md bg-emerald-100 px-2.5 py-1 text-xs font-bold text-emerald-700">{gig.status}</span>
					</div>
				</div>
			</div>
		{/each}
	</div>
</div>  -->
</div>
<!-- ================= ADD NEW SERVICE POPUP MODAL ================= -->
{#if isModalOpen}
	<!-- Backdrop Blur Overlay -->
	<div class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-4 backdrop-blur-sm">
		
		<!-- Modal Content Box -->
		<div class="w-full max-w-lg rounded-3xl bg-white p-6 shadow-2xl transition-all">
			
			<!-- Modal Header -->
			<div class="flex items-center justify-between border-b border-slate-100 pb-4">
				<h3 class="text-xl font-bold text-slate-900">Create New Service</h3>
				<button
					onclick={() => (isModalOpen = false)}
					class="rounded-full p-1 text-slate-400 hover:bg-slate-100 hover:text-slate-600"
				>
					✕
				</button>
			</div>

			<!-- Form Inside Popup -->
			<form onsubmit={services} class="mt-4 space-y-4">
				<!-- Title Input -->
				<div>
					<label for="gig-title" class="block text-xs font-bold uppercase text-slate-600">Service Title</label>
					<input
						id="gig-title"
						type="text"
						bind:value={title}
						placeholder="e.g. I will build a full-stack SvelteKit web app"
						required
						class="mt-1 w-full rounded-xl border border-slate-200 bg-slate-50 p-3 text-sm font-medium focus:border-blue-600 focus:bg-white focus:outline-none"
					/>
				</div>

				<!-- Category & Price Grid -->
				<div class="grid grid-cols-2 gap-4">
					<div>
						<label for="gig-category" class="block text-xs font-bold uppercase text-slate-600">Category</label>
						<select
							id="gig-category"
							bind:value={category}
							
							class="mt-1 w-full rounded-xl border border-slate-200 bg-slate-50 p-3 text-sm font-medium focus:border-blue-600 focus:bg-white focus:outline-none"
						
						>
						<option >Web Development</option>
							<option >UI/UX Design</option>
							<option >Digital Marketing</option>
							<option >AI Development</option>
						</select>
					</div>

					<div>
						<label for="gig-price" class="block text-xs font-bold uppercase text-slate-600">Starting Price ($)</label>
						<input
							id="gig-price"
							type="number"
							bind:value={price}
							placeholder="150"
							required
							class="mt-1 w-full rounded-xl border border-slate-200 bg-slate-50 p-3 text-sm font-medium focus:border-blue-600 focus:bg-white focus:outline-none"
						/>
					</div>
				</div>

				<!-- Description Input -->
				<div>
					<label for="gig-description" class="block text-xs font-bold uppercase text-slate-600">Service Description</label>
					<textarea
						id="gig-description"
						bind:value={description}
						rows="3"
						placeholder="Explain what features you offer..."
						required
						class="mt-1 w-full rounded-xl border border-slate-200 bg-slate-50 p-3 text-sm font-medium focus:border-blue-600 focus:bg-white focus:outline-none"
					></textarea>
				</div>

				<!-- Modal Actions -->
				<div class="mt-6 flex items-center justify-end gap-3 pt-2">
					<button
						type="button"
						onclick={() => (isModalOpen = false)}
						class="rounded-xl px-4 py-2.5 text-sm font-bold text-slate-600 hover:bg-slate-100"
					>
						Cancel
					</button>
					<button
						type="submit"
						class="rounded-xl bg-blue-600 px-5 py-2.5 text-sm font-bold text-white shadow-md hover:bg-blue-700"
					>
						Publish Service
					</button>
				</div>
			</form>

		</div>
	</div>
{/if}
<!-- ================= MODERN SERVICES GRID SYSTEM ================= -->
<main class="mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8 font-sans">
	
	<!-- Section Header -->
	<div class="mb-10 text-center md:text-left">
	
		
	</div>

	{#if !data || !data.services}
		<!-- Loading Skeleton Animation -->
		<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
			{#each Array(3) as _}
				<div class="animate-pulse rounded-3xl bg-slate-100 h-96 w-full"></div>
			{/each}
		</div>
	{:else if data.services.length === 0}
		<!-- Empty State Screen -->
		<div class="rounded-3xl border border-dashed border-slate-200 bg-slate-50/50 py-16 text-center">
			<svg class="mx-auto h-12 w-12 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4" />
			</svg>
			<h3 class="mt-4 text-sm font-bold text-slate-900">No data found</h3>
			<p class="mt-1 text-sm text-slate-500">Upload a service.</p>
		</div>
	{:else}
		<!-- Responsive Cards Grid: Mobile me 1, Tablet me 2, Desktop me 3 columns -->
		<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">
			{#each data.services as item}
				<div class="group relative flex flex-col justify-between overflow-hidden rounded-3xl border border-slate-200 bg-white p-2 shadow-sm transition-all duration-300 hover:-translate-y-1 hover:shadow-md">
					
					<div>
						<!-- Service Display Image (Dynamic placeholder matching the niche) -->
						<div class="relative h-48 w-full overflow-hidden rounded-2xl bg-slate-100">
							<img 
								src="" 
								alt={item.title} 
								class="h-full w-full object-cover transition-transform duration-500 group-hover:scale-105" 
							/>
							<!-- Category Floating Tag -->
							<span class="absolute top-3 left-3 inline-flex items-center rounded-xl bg-white/90 px-3 py-1 text-xs font-black uppercase tracking-wider text-blue-600 shadow-sm backdrop-blur-xs">
								{item.category || 'General'}
							</span>
						</div>

						<!-- Card Body Text Details -->
						<div class="px-3 pt-4 pb-2">
							<h3 class="text-xl font-extrabold tracking-tight text-slate-800 line-clamp-2 group-hover:text-blue-600 transition-colors">
								{item.title}
							</h3>
							<p class="mt-2 text-sm font-medium leading-relaxed text-slate-500 line-clamp-3">
								{item.description || 'No description provided for this specific package.'}
							</p>
						</div>
					</div>

					<!-- Card Footer Container -->
					<div class="mt-4 flex items-center justify-between border-t border-slate-100 px-3 pt-3 pb-1">
						<div class="flex flex-col">
							<span class="text-[10px] font-bold uppercase tracking-wider text-slate-400">Starting At</span>
							<span class="text-2xl font-black text-slate-900">${item.price}</span>
						</div>
						
						<!-- Action Button for Users -->
						<button class="inline-flex h-10 items-center justify-center rounded-xl bg-slate-900 px-4 text-xs font-bold text-white transition hover:bg-blue-600 active:scale-95">
							View Details
						</button>
					</div>

				</div>
			{/each}
		</div>
	{/if}
</main>

