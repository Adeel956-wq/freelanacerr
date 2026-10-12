<script>
	// Categories Data Array
	const categories = [
		{
			title: 'Web & Software Dev',
			description: 'Frontend, Backend, Full-stack & Mobile Apps',
			icon: '💻'
		},
		{
			title: 'Design & Creative',
			description: 'Logos, UI/UX Design, Branding & Illustration',
			icon: '🎨'
		},
		{
			title: 'Writing & Translation',
			description: 'Articles, Proofreading, Copywriting & Translations',
			icon: '✍️'
		},
		{
			title: 'Digital Marketing',
			description: 'SEO, Social Media, Content Strategy & Ads',
			icon: '📈'
		},
		{
			title: 'Video & Animation',
			description: 'Video Editing, Motion Graphics & 3D Visuals',
			icon: '🎥'
		},
		{
			title: 'Music & Audio',
			description: 'Voice Overs, Mixing, Mastering & Sound Design',
			icon: '🎧'
		},
		{
			title: 'Finance & Business',
			description: 'Business Plans, Accounting & Market Research',
			icon: '📊'
		},
		{
			title: 'AI & Data Science',
			description: 'Machine Learning, Data Entry & AI Prompting',
			icon: '🤖'
		}
	];

	let searchQuery = $state('');

	function handleSearch(event) {
		event.preventDefault();
		if (searchQuery.trim()) {
			console.log('Searching for:', searchQuery);
		}
	}
</script>

<div class="min-h-screen bg-slate-50 font-sans text-slate-800">
	<!-- Header Navigation -->
	<nav class="flex items-center justify-between bg-white px-8 py-5 shadow-sm">
		<div class="text-xl font-bold text-blue-600">Project Freelancer</div>
		<ul class="hidden items-center gap-8 font-medium text-slate-600 md:flex">
			<li><a href="#find-work" class="transition hover:text-blue-600">Find Work</a></li>
			<li><a href="#hire-talent" class="transition hover:text-blue-600">Hire Talent</a></li>
			<li><a href="#categories" class="transition hover:text-blue-600">Categories</a></li>
		</ul>
		<div class="flex items-center gap-4">
			<a
				href="#login"
				class="rounded-lg border border-blue-600 px-4 py-2 font-semibold text-blue-600 transition hover:bg-blue-50"
			>
				Log In
			</a>
			<a
				href="/createAccount"
				class="rounded-lg bg-blue-600 px-4 py-2 font-semibold text-white transition hover:bg-blue-700"
			>
				Sign Up
			</a>
		</div>
	</nav>

	<!-- Hero Section -->
	<header class="bg-gradient-to-r from-blue-900 to-blue-600 px-4 py-20 text-center text-white">
		<h1 class="mb-4 text-4xl font-extrabold md:text-5xl">
			Hire Top Freelancers for Any Job, Online
		</h1>
		<p class="mb-8 text-lg text-blue-100 md:text-xl">
			Connect with millions of experts worldwide to transform your ideas into reality.
		</p>

		<!-- Search Bar Form -->
		<form
			onsubmit={handleSearch}
			class="mx-auto flex max-w-xl overflow-hidden rounded-lg bg-white shadow-lg"
		>
			<input
				type="text"
				bind:value={searchQuery}
				placeholder="What service are you looking for?"
				class="w-full px-5 py-3 text-slate-700 outline-none placeholder:text-slate-400"
			/>
			<button
				type="submit"
				class="bg-slate-900 px-6 font-semibold text-white transition hover:bg-slate-800"
			>
				Search
			</button>
		</form>
	</header>

	<!-- Categories Section -->
	<main class="mx-auto max-w-7xl px-6 py-16">
		<h2 class="mb-10 text-center text-3xl font-bold text-slate-800">
			Explore Popular Categories
		</h2>

		<div class="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-4">
			{#each categories as category}
				<div
					class="group cursor-pointer rounded-xl border border-slate-200 bg-white p-6 text-center shadow-sm transition hover:-translate-y-1 hover:shadow-md"
				>
					<div class="mb-4 text-4xl">{category.icon}</div>
					<h3 class="mb-2 text-lg font-bold text-slate-800 group-hover:text-blue-600">
						{category.title}
					</h3>
					<p class="text-sm text-slate-500">
						{category.description}
					</p>
				</div>
			{/each}
		</div>
	</main>

	<!-- Footer -->
	<footer class="mt-12 bg-slate-900 py-8 text-center text-sm text-slate-400">
		<p>&copy; 2026 Project Freelancer. All rights reserved.</p>
	</footer>
</div>