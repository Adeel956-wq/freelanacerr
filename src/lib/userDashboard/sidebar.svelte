<script lang="ts">
	// Svelte 5 bindable props
	let { isSidebarOpen = $bindable(false), activeSection = $bindable('overview') } = $props();

	function handleLogout() {
		localStorage.removeItem('access_token');
		localStorage.removeItem('user_data');
		window.location.href = '/login';
	}
</script>

<!-- Mobile Dark Overlay (Backdrop) -->
{#if isSidebarOpen}
	<button 
		type="button"
		aria-label="Close sidebar overlay"
		onclick={() => (isSidebarOpen = false)}
		class="fixed inset-0 z-40 bg-slate-900/50 backdrop-blur-sm transition-opacity lg:hidden"
	></button>
{/if}

<!-- Sidebar Container -->
<aside
	class={`fixed inset-y-0 left-0 z-50 flex flex-col border-r border-slate-200 bg-white transition-all duration-300 ease-in-out lg:static ${
		isSidebarOpen 
			? 'translate-x-0 w-64' 
			: '-translate-x-full lg:translate-x-0 lg:w-20'
	}`}
>
	<!-- Sidebar Header Logo -->
	<div class="flex h-16 shrink-0 items-center justify-between border-b border-slate-100 px-4">
		<a href="/" class="flex items-center gap-3 overflow-hidden">
			<span class="flex h-10 w-10 shrink-0 items-center justify-center rounded-xl bg-blue-600 text-lg font-black text-white shadow-md shadow-blue-500/20">
				P
			</span>
			{#if isSidebarOpen}
				<span class="truncate text-lg font-black tracking-tight text-slate-900">Project<span class="text-blue-600">Freelance</span></span>
			{/if}
		</a>

		<!-- Mobile Close Button -->
		<button
			onclick={() => (isSidebarOpen = false)}
			class="rounded-lg p-1.5 text-slate-500 hover:bg-slate-100 lg:hidden"
			aria-label="Close Sidebar"
		>
			<svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
			</svg>
		</button>
	</div>

	<!-- Sidebar Navigation Items -->
	<nav class="flex-1 space-y-1.5 overflow-y-auto p-3">
		<button
			onclick={() => { activeSection = 'overview'; isSidebarOpen = false; }}
			class={`flex w-full items-center gap-3.5 rounded-xl px-3.5 py-3 font-semibold transition ${
				activeSection === 'overview' ? 'bg-blue-600 text-white shadow-md shadow-blue-600/20' : 'text-slate-600 hover:bg-slate-100'
			}`}
		>
			<svg class="h-5 w-5 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 00-1-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
			</svg>
			<span class={`truncate ${!isSidebarOpen && 'lg:hidden'}`}>Dashboard</span>
		</button>

		<button
			onclick={() => { activeSection = 'orders'; isSidebarOpen = false; }}
			class={`flex w-full items-center gap-3.5 rounded-xl px-3.5 py-3 font-semibold transition ${
				activeSection === 'orders' ? 'bg-blue-600 text-white shadow-md shadow-blue-600/20' : 'text-slate-600 hover:bg-slate-100'
			}`}
		>
			<svg class="h-5 w-5 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
			</svg>
			<span class={`truncate ${!isSidebarOpen && 'lg:hidden'}`}>My Orders</span>
		</button>

		<button
			onclick={() => { activeSection = 'services'; isSidebarOpen = false; }}
			class={`flex w-full items-center gap-3.5 rounded-xl px-3.5 py-3 font-semibold transition ${
				activeSection === 'services' ? 'bg-blue-600 text-white shadow-md shadow-blue-600/20' : 'text-slate-600 hover:bg-slate-100'
			}`}
		>
			<svg class="h-5 w-5 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 13.255A23.931 23.931 0 0112 15c-3.183 0-6.22-.62-9-1.745M16 6V4a2 2 0 00-2-2h-4a2 2 0 00-2 2v2m4 6h.01M5 20h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
			</svg>
			<span class={`truncate ${!isSidebarOpen && 'lg:hidden'}`}>My Services</span>
		</button>

		<button
			onclick={() => { activeSection = 'messages'; isSidebarOpen = false; }}
			class={`flex w-full items-center gap-3.5 rounded-xl px-3.5 py-3 font-semibold transition ${
				activeSection === 'messages' ? 'bg-blue-600 text-white shadow-md shadow-blue-600/20' : 'text-slate-600 hover:bg-slate-100'
			}`}
		>
			<svg class="h-5 w-5 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 10h.01M12 10h.01M16 10h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
			</svg>
			<span class={`truncate ${!isSidebarOpen && 'lg:hidden'}`}>Messages</span>
		</button>
	</nav>

	<!-- Sidebar Logout Button -->
	<div class="shrink-0 border-t border-slate-100 p-3">
		<button
			onclick={handleLogout}
			class="flex w-full items-center gap-3.5 rounded-xl px-3.5 py-3 font-semibold text-red-600 transition hover:bg-red-50"
		>
			<svg class="h-5 w-5 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor">
				<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
			</svg>
			<span class={`truncate ${!isSidebarOpen && 'lg:hidden'}`}>Sign Out</span>
		</button>
	</div>
</aside>