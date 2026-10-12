<script lang="ts">
	import { onMount } from 'svelte';

	let isMobileMenuOpen = $state(false);
	let isProfileMenuOpen = $state(false);

	// Authenticated state and dynamic user profile data
	let isLoggedIn = $state();
	let user = $state({
		name: '',
		email: '',
		avatarUrl: ''
	});

	onMount(async () => {
		// 1. LocalStorage se token / user session detail fetch karein
		const token = localStorage.getItem('access_token');
		const storedUser = localStorage.getItem('user_data');

		if (storedUser) {
			try {
				const parsed = JSON.parse(storedUser);
				user = {
					name: parsed.full_name || parsed.name || '',
					email: parsed.email || '',
					avatarUrl: parsed.avatar_url || ''
				};
				isLoggedIn = true;
			} catch (e) {
				console.error("User data parsing error:", e);
			}
		} else if (token) {
			// Agar direct user details localstorage me nahi hain to token check karein
			isLoggedIn = true;
		}
	});

	function toggleProfileMenu() {
		isProfileMenuOpen = !isProfileMenuOpen;
	}

	function handleLogout() {
		localStorage.removeItem('access_token');
		localStorage.removeItem('user_data');
		isLoggedIn = false;
		user = { name: '', email: '', avatarUrl: '' };
		window.location.href = '/login';
	}

	// Dynamic User Profile Initials function (e.g. "Ahmad Ali" -> "AA")
	function getInitials(name: string, email: string) {
		if (name) {
			const parts = name.trim().split(' ');
			if (parts.length >= 2) return `${parts[0][0]}${parts[1][0]}`.toUpperCase();
			return name.slice(0, 2).toUpperCase();
		}
		return email ? email.slice(0, 2).toUpperCase() : 'U';
	}
</script>

<nav class="sticky top-0 z-50 border-b border-slate-200/80 bg-white/95 backdrop-blur-md">
	<div class="mx-auto flex max-w-7xl items-center justify-between px-4 py-3.5 sm:px-6 lg:px-8">
		<!-- Brand Logo -->
		<a href="/" class="flex items-center gap-2 text-2xl font-black tracking-tight text-slate-900">
			<span class="flex h-9 w-9 items-center justify-center rounded-xl bg-blue-600 text-lg text-white shadow-md shadow-blue-500/20">
				P
			</span>
			<span>Project<span class="text-blue-600">Freelance</span></span>
		</a>

		<!-- Navigation Links -->
		<div class="hidden items-center gap-8 font-medium text-slate-600 lg:flex">
			<a href="#explore" class="transition hover:text-blue-600">Explore</a>
			<a href="#hire" class="transition hover:text-blue-600">Find Talent</a>
			<a href="#work" class="transition hover:text-blue-600">Find Work</a>
			<a href="#enterprise" class="transition hover:text-blue-600">Enterprise Solution</a>
		</div>

		<!-- Action Buttons / Dynamic Profile Circle -->
		<div class="hidden items-center gap-3 md:flex">
			{#if isLoggedIn}
				<!-- Logged In State: Login aur Join Buttons REMOVE ho kar Profile Circle aa jayega -->
				<div class="relative">
					<button
						onclick={toggleProfileMenu}
						class="relative flex h-10 w-10 items-center justify-center rounded-full bg-slate-900 text-sm font-bold text-white transition ring-2 ring-transparent hover:ring-blue-600 focus:outline-none cursor-pointer"
						aria-label="User menu"
					>
						{#if user.avatarUrl}
							<img src={user.avatarUrl} alt={user.name} class="h-full w-full rounded-full object-cover" />
						{:else}
							<span>{getInitials(user.name, user.email)}</span>
						{/if}
						
						<!-- Status indicator -->
						<span class="absolute bottom-0 right-0 h-2.5 w-2.5 rounded-full bg-emerald-500 ring-2 ring-white"></span>
					</button>

					<!-- Profile Dropdown Menu -->
					{#if isProfileMenuOpen}
						<div class="absolute right-0 mt-2 w-56 rounded-xl border border-slate-100 bg-white p-2 shadow-lg ring-1 ring-black/5 animate-in fade-in slide-in-from-top-2">
							<div class="border-b border-slate-100 px-3 py-2.5">
								<p class="text-sm font-semibold text-slate-900">{user.name || 'User Account'}</p>
								<p class="truncate text-xs text-slate-500">{user.email}</p>
							</div>
							
							<div class="py-1 text-sm text-slate-700">
								<a href="/dashboard" class="flex items-center gap-2 rounded-lg px-3 py-2 hover:bg-slate-50 transition-colors">
									<svg class="h-4 w-4 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 00-1-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
									</svg>
									Dashboard
								</a>
								<a href="/profile" class="flex items-center gap-2 rounded-lg px-3 py-2 hover:bg-slate-50 transition-colors">
									<svg class="h-4 w-4 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
									</svg>
									My Profile
								</a>
							</div>

							<div class="border-t border-slate-100 pt-1">
								<button
									onclick={handleLogout}
									class="flex w-full items-center gap-2 rounded-lg px-3 py-2 text-left text-sm font-medium text-red-600 hover:bg-red-50 transition-colors cursor-pointer"
								>
									<svg class="h-4 w-4 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
										<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
									</svg>
									Sign Out
								</button>
							</div>
						</div>
					{/if}
				</div>
			{:else}
				<!-- Logged Out State -->
				<a href="/login" class="rounded-lg px-4 py-2 font-semibold text-slate-700 transition hover:text-blue-600">
					Log In
				</a>
				<a
					href="/createAccount"
					class="rounded-lg bg-blue-600 px-5 py-2.5 font-semibold text-white shadow-sm transition hover:bg-blue-700 hover:shadow-blue-600/25"
				>
					Join Marketplace
				</a>
			{/if}
		</div>

		<!-- Mobile Menu Button -->
		<button
			onclick={() => (isMobileMenuOpen = !isMobileMenuOpen)}
			class="rounded-lg p-2 text-slate-600 hover:bg-slate-100 lg:hidden"
			aria-label="Toggle Navigation"
		>
			<svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
				{#if isMobileMenuOpen}
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
				{:else}
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
				{/if}
			</svg>
		</button>
	</div>

	<!-- Mobile Menu Panel -->
	{#if isMobileMenuOpen}
		<div class="border-b border-slate-200 bg-white px-6 py-4 lg:hidden">
			<div class="flex flex-col gap-4 font-medium text-slate-700">
				<a href="#explore" class="hover:text-blue-600">Explore</a>
				<a href="#hire" class="hover:text-blue-600">Find Talent</a>
				<a href="#work" class="hover:text-blue-600">Find Work</a>
				
				{#if isLoggedIn}
					<div class="border-t border-slate-200 pt-3 flex flex-col gap-2">
						<span class="text-xs font-semibold text-slate-400">LOGGED IN AS</span>
						<p class="font-semibold text-slate-900">{user.email}</p>
						<a href="/dashboard" class="text-slate-700 hover:text-blue-600">Dashboard</a>
						<a href="/profile" class="text-slate-700 hover:text-blue-600">My Profile</a>
						<button onclick={handleLogout} class="text-left font-semibold text-red-600 pt-2">Sign Out</button>
					</div>
				{:else}
					<div class="pt-2 flex flex-col gap-2">
						<a href="/login" class="w-full text-center rounded-lg border border-slate-300 py-2.5 font-semibold">Log In</a>
						<a href="/createAccount" class="w-full text-center rounded-lg bg-blue-600 py-2.5 font-semibold text-white">Join Marketplace</a>
					</div>
				{/if}
			</div>
		</div>
	{/if}
</nav>