<script lang="ts">
	import { onMount } from 'svelte';
	import Sidebar from '$lib/userDashboard/sidebar.svelte';
	import Services from '$lib/userDashboard/services.svelte';

	let isSidebarOpen = $state(false);
	let activeSection = $state('overview');
	let isProfileDropdownOpen = $state(false);
	let isLoading = $state(true);

	let user = $state({
		name: '',
		email: '',
		avatarUrl: ''
	});

	onMount(() => {
		const storedUser = localStorage.getItem('user_data');
		const token = localStorage.getItem('access_token');

		//if (!token) { window.location.href = '/login'; return; }

		if (storedUser) {
			try {
				const parsed = JSON.parse(storedUser);
				user.email = parsed.email || '';
				user.name = parsed.name || parsed.user_metadata?.full_name || user.email.split('@')[0];
				user.avatarUrl = parsed.user_metadata?.avatar_url || '';
			} catch (e) {
				console.error('Error loading user data', e);
			}
		}
		//isLoading = false;
		let progress = $state(0);
	//let isLoading = $state(true);

	onMount(() => {
		// Smooth fill animation start karna
		setTimeout(() => { progress = 100; }, 50);

		// 1 second baad loader hide karna
		setTimeout(() => { isLoading = false; }, 1000);
	});
	});

	function getInitials(name: string, email: string) {
		if (name) {
			const parts = name.trim().split(' ');
			if (parts.length >= 2) return `${parts[0][0]}${parts[1][0]}`.toUpperCase();
			return name.slice(0, 2).toUpperCase();
		}
		return email ? email.slice(0, 2).toUpperCase() : 'U';
	}

	function handleLogout() {
		localStorage.removeItem('access_token');
		localStorage.removeItem('user_data');
		window.location.href = '/login';
	}
</script>

<svelte:head>
	<title>User Dashboard | Project Freelancer</title>
</svelte:head>

{#if isLoading}
	<!-- Full Black Screen Center Loader -->
	<div class="fixed inset-0 z-50 flex flex-col items-center justify-center bg-black px-6">
		<div class="w-full max-w-md space-y-3 text-center">
			
			<!-- Center Linear Progress Indicator Container -->
			<div class="h-2 w-full overflow-hidden rounded-full bg-slate-800">
				<div 
					class="h-full bg-blue-600 transition-all duration-1000 ease-out" 
					style="width:%;"
				></div>
			</div>

			<!-- Loading Text (Optional) -->
			<p class="text-xs font-semibold tracking-wider text-slate-400 uppercase">Loading...</p>
			
		</div>
	</div>
{:else}
	<div class="flex h-screen overflow-hidden bg-slate-50 font-sans text-slate-800">

		<!-- Sidebar Component -->
		<Sidebar bind:isSidebarOpen bind:activeSection />

		<!-- Main Content Area -->
		<div class="flex flex-1 flex-col overflow-y-auto">

			<!-- Top Navbar Header -->
			<header class="sticky top-0 z-30 flex h-16 items-center justify-between border-b border-slate-200 bg-white/95 px-6 backdrop-blur-md">
				<div class="flex items-center gap-4">
					<button
						onclick={() => (isSidebarOpen = !isSidebarOpen)}
						class="rounded-lg p-2 text-slate-600 hover:bg-slate-100"
						aria-label="Toggle sidebar"
					>
						<svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
							<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
						</svg>
					</button>
					<h2 class="text-lg font-bold text-slate-800 capitalize">{activeSection}</h2>
				</div>

				<!-- User Profile Avatar & Dropdown -->
				<div class="relative">
					<button
						onclick={() => (isProfileDropdownOpen = !isProfileDropdownOpen)}
						class="relative flex h-10 w-10 items-center justify-center rounded-full bg-slate-900 text-sm font-bold text-white shadow hover:ring-2 hover:ring-blue-600"
					>
						{#if user.avatarUrl}
							<img src={user.avatarUrl} alt={user.name} class="h-full w-full rounded-full object-cover" />
						{:else}
							<span>{getInitials(user.name, user.email)}</span>
						{/if}
						<span class="absolute bottom-0 right-0 h-2.5 w-2.5 rounded-full bg-emerald-500 ring-2 ring-white"></span>
					</button>

					{#if isProfileDropdownOpen}
						<div class="absolute right-0 mt-2 w-56 rounded-2xl border border-slate-100 bg-white p-2 shadow-xl">
							<div class="border-b border-slate-100 px-3 py-2">
								<p class="text-sm font-bold text-slate-900">{user.name}</p>
								<p class="truncate text-xs text-slate-500">{user.email}</p>
							</div>
							<button
								onclick={handleLogout}
								class="flex w-full items-center gap-2 rounded-lg px-3 py-2 text-sm font-semibold text-red-600 hover:bg-red-50"
							>
								Sign Out
							</button>
						</div>
					{/if}
				</div>
			</header>

			<!-- Dynamic Content Body based on activeSection -->
			<main class="flex-1 p-6">
				{#if activeSection === 'overview'}
					<div class="rounded-2xl border border-blue-100 bg-gradient-to-r from-blue-50 to-white p-6 shadow-sm">
						<h1 class="text-2xl font-black text-slate-900">Welcome, {user.name}!</h1>
						<p class="mt-1 text-sm text-slate-600">Select options from the sidebar to manage your account.</p>
					</div>

				{:else if activeSection === 'orders'}
					<div>
						<h1 class="text-2xl font-bold text-slate-900">My Orders</h1>
						<p class="text-slate-600">Here you can view and manage all your active/completed orders.</p>
						<div class="mt-4 rounded-xl border border-slate-200 bg-white p-4 shadow-sm">
							<p class="text-slate-500">No orders found yet.</p>
						</div>
					</div>

				{:else if activeSection === 'services'}
					<Services />

				{:else if activeSection === 'messages'}
					<div>
						<h1 class="text-2xl font-bold text-slate-900">Messages</h1>
						<p class="text-slate-600">View your inbox and customer chats.</p>
					</div>
				{/if}
			</main>

		</div>
	</div>
{/if}