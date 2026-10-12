<script lang="ts">
	let email: string = $state("");
	let password: string = $state("");
	let rememberMe: boolean = $state(false);
	let showPassword: boolean = $state(false);
	
	let errorMessage: string | null = $state(null);
	let isLoading: boolean = $state(false);

	async function handleLogin(e:any): Promise<void> {
    e.preventDefault();
		errorMessage =null;
		isLoading = true;

		try {
			const response = await fetch('http://192.168.0.172:8080/login', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ email, password })
			});

			const data = await response.json();
console.log("error is : ",data);
			if (!response.ok) {
		 const errorMsg =  'Invalid email or password';
				throw new Error(errorMsg);
				
			}

			// Store session context or auth tokens securely
			if(data.access_token || data.user) {
				localStorage.setItem('access_token', data.access_token);
				localStorage.setItem('user_data', JSON.stringify(data.user));
			
			}
			// Navigate upon success
			window.location.href = '/';
		} catch (err) {
			if (err instanceof Error) {
				errorMessage = err.message;
			} else {
				errorMessage = 'An unexpected error occurred';
			}
		} finally {
			isLoading = false;
		}
	}
</script>

<div class="flex min-h-screen flex-col justify-between bg-slate-950 font-sans text-slate-100 selection:bg-blue-600 selection:text-white antialiased">
	<header class="mx-auto flex w-full max-w-7xl items-center justify-between px-6 py-6">
		<a href="/" class="group flex items-center gap-2.5">
			<span class="flex h-8 w-8 items-center justify-center rounded-lg bg-blue-600 text-sm font-bold text-white shadow-sm transition-transform group-hover:scale-105">
				P
			</span>
			<span class="text-base font-bold tracking-tight text-white">Project<span class="text-blue-500">Freelance</span></span>
		</a>
		<span class="text-xs text-slate-400 sm:text-sm">
			Need an account? 
			<a href="/createAccount" class="font-medium text-blue-400 hover:text-blue-300 transition-colors underline underline-offset-4">
				Create account
			</a>
		</span>
	</header>

	<main class="mx-auto w-full max-w-sm px-6 py-8">
		<div class="mb-8">
			<h1 class="text-2xl font-bold tracking-tight text-white">Sign in to platform</h1>
			<p class="mt-1.5 text-xs text-slate-400 sm:text-sm">Enter your credentials to access your workspace.</p>
		</div>

		{#if errorMessage}
			<div class="mb-4 rounded-lg bg-red-950/50 border border-red-800 p-3 text-xs text-red-400">
				{errorMessage}
			</div>
		{/if}

		<form onsubmit={handleLogin} class="space-y-4">
			<div>
				<label for="email" class="mb-1.5 block font-mono text-[11px] font-semibold uppercase tracking-wider text-slate-400">
					Email Address
				</label>
				<input
					id="email"
					type="email"
					required
					bind:value={email}
					placeholder="name@email.com"
					class="w-full rounded-lg border border-slate-800 bg-slate-900 px-3.5 py-2.5 text-sm text-slate-100 placeholder-slate-600 transition-colors focus:border-blue-500 focus:bg-slate-900 focus:outline-none focus:ring-1 focus:ring-blue-500"
				/>
			</div>

			<div>
				<div class="mb-1.5 flex items-center justify-between">
					<label for="password" class="block font-mono text-[11px] font-semibold uppercase tracking-wider text-slate-400">
						Password
					</label>
					<a href="/forgot-password" class="text-xs text-slate-400 hover:text-slate-200 transition-colors">
						Forgot?
					</a>
				</div>
				<div class="relative">
					<input
						id="password"
						type={showPassword ? 'text' : 'password'}
						required
						bind:value={password}
						placeholder="••••••••••••"
						class="w-full rounded-lg border border-slate-800 bg-slate-900 pr-10 pl-3.5 py-2.5 text-sm text-slate-100 placeholder-slate-600 transition-colors focus:border-blue-500 focus:bg-slate-900 focus:outline-none focus:ring-1 focus:ring-blue-500"
					/>
					<button
						type="button"
						onclick={() => (showPassword = !showPassword)}
						class="absolute right-3 top-1/2 -translate-y-1/2 font-mono text-[10px] font-bold text-slate-500 hover:text-slate-300 transition-colors"
					>
						{showPassword ? 'HIDE' : 'SHOW'}
					</button>
				</div>
			</div>

			<div class="flex items-center pt-1">
				<input
					id="remember"
					type="checkbox"
					bind:checked={rememberMe}
					class="h-4 w-4 rounded border-slate-800 bg-slate-900 text-blue-600 focus:ring-blue-500 focus:ring-offset-slate-950"
				/>
				<label for="remember" class="ml-2.5 text-xs text-slate-400 select-none">
					Keep me signed in
				</label>
			</div>

			<button
				type="submit"
				disabled={isLoading}
				class="mt-2 w-full rounded-lg bg-blue-600 px-4 py-2.5 text-sm font-semibold text-white shadow-sm transition-colors hover:bg-blue-500 active:bg-blue-700 disabled:opacity-50"
			>
				{isLoading ? 'Signing in...' : 'Sign In'}
			</button>
		</form>
	</main>

	<footer class="mx-auto w-full max-w-7xl px-6 py-6 text-center text-xs text-slate-600">
		Protected by security standards. Privacy Policy & Terms apply.
	</footer>
</div>