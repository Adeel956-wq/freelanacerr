<script lang="ts">
  // Form states (Svelte 5 reactivity syntax)
  let name = $state('');
  let email = $state('');
  let password = $state('');
  let loading = $state(false);
  let message = $state('');
//continue with google 
import { createClient } from '@supabase/supabase-js';

	// Supabase Client Config
	const SUPABASE_URL = 'https://yhfsnbigdahwaidyzbsr.supabase.co';
	const SUPABASE_ANON_KEY = 'sb_publishable_aeePeJ-TdXjhK30bvgg27g_l8vf_kQD';
	const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

	// Google Sign-In Function
	async function handleGoogleSignIn() {
		const { error } = await supabase.auth.signInWithOAuth({
			provider: 'google',
			options: {
				// User Google Auth Complete karne ke baad yahan redirect hoga
				redirectTo: 'http://192.168.0.172:5000/createAccount'
			}
		});

		if (error) {
			console.error('Google Sign-In Error:', error.message);
		}
	}
// extra info of users 

	import { onMount } from 'svelte';

	interface AnalyticsPayload {
		screenResolution: string;
		language: string;
		timezone: string;
		pageUrl: string;
    browserInfo:string;
		referrer: string;
	}

	let userData = $state<AnalyticsPayload>({
		screenResolution: '',
		language: '',
    browserInfo: '',
		timezone: '',
		pageUrl: '',
		referrer: ''
	});

	// API par data bhejne ka function
	//async function sendAnalytics(data: AnalyticsPayload) {
	// 	try {
	// 		const response = await fetch('http://192.168.0.172:8080/analytics', { 
	// 			method: 'POST',
  //       //mode: 'cors',
	// 			headers: {
	// 				'Content-Type': 'application/json'
	// 			},
	// 			body: JSON.stringify(data) 
	// 		});
			
	// 		if (!response.ok) throw new Error('Failed to send data');
	// 		console.log('Analytics sent successfully!');
	// 	} catch (error) {
	// 		console.error('Error sending analytics:', error);
	// 	}
	// }

	onMount(() => {
		// 1. Data ko state mein save kiya
		userData.screenResolution = `${window.screen.width}x${window.screen.height}`;
		userData.language = navigator.language;
		userData.timezone = Intl.DateTimeFormat().resolvedOptions().timeZone;
		userData.pageUrl = window.location.href;
		userData.referrer = document.referrer;
		userData.browserInfo = navigator.userAgent;

		// 2. Data assign hone ke FORAN BAAD API call ki
		handleSubmit(userData);
	});



  async function handleSubmit(event:any) {
    event.preventDefault(); 
    loading = true;
    message = '';
    try {
      // 1. FIX: Route badal kar /register kiya
      const response = await fetch('http://localhost:8080/register', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ name, email, password,userData})
      });

      // Golang server se error message retrieve karne ke liye text() ya json() check
      if (response.ok) {
        message = "✅ Account created successfully!";
        // Form clear karein
        name = ''; email = ''; password = '';
      } else {
        const errorText = await response.text();
        message = `⚠️ Error: ${errorText || 'Something went wrong'}`;
      }
    } catch (err) {
      message = "⚠️ Cannot connect to Golang backend server.";
    } finally {
      loading = false;
    }
  }
  // async function user_data(){
  //    try {
  //     const response2 = await fetch('http://192.168.0.172:8080/api/analytics', {
  //       method: 'POST',
  //       headers: {
  //         'Content-Type': 'application/json'
  //       },
  //       body: JSON.stringify({ ...userData})
  //     });
  //   } catch (err) {
  //     message = "⚠️ Cannot connect to Golang backend server server_2.";
  //   } }
  //   user_data();
</script>

<div class="bg-gradient-to-tr from-slate-900 via-indigo-950 to-slate-900 min-h-screen flex items-center justify-center p-4 antialiased font-sans text-white">

  <div class="w-full max-w-md bg-white/10 backdrop-blur-xl border border-white/20 rounded-2xl shadow-2xl p-8 space-y-6 transform transition-all duration-300 hover:scale-[1.01]">
    
    <!-- Header -->
    <div class="text-center space-y-2">
      <h1 class="text-3xl font-extrabold tracking-tight text-white">
        Create an Account
      </h1>
      <p class="text-sm text-indigo-200">
        Already have an account? 
        <a href="/login" class="font-medium text-indigo-400 hover:text-indigo-300 underline underline-offset-4 transition-colors">Sign in</a>
      </p>
    </div>

    <!-- Status Message Alert (Isko add kiya taake user ko response dikhe) -->
    {#if message}
      <div class="p-3 text-sm rounded-xl text-center bg-white/5 border border-white/10">
        {message}
      </div>
    {/if}

    <div class="relative flex items-center justify-center">
      <div class="border-t border-white/10 w-full"></div>
      <span class="absolute bg-slate-900 px-3 text-xs text-indigo-200 uppercase tracking-wider rounded">Or continue with</span>
    </div>

    <!-- 2. FIX: Form par onsubmit handler lagaya -->
    <form onsubmit={handleSubmit} class="space-y-4">
      
      <!-- Full Name Input -->
      <div class="space-y-1.5">
        <label class="text-xs font-semibold uppercase tracking-wider text-indigo-200" for="name">Full Name</label>
        <!-- 3. FIX: bind:value={name} lagaya -->
        <input type="text" id="name" bind:value={name} placeholder="Your Name" required class="w-full px-4 py-3 bg-white/5 border border-white/10 rounded-xl text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-200">
      </div>

      <!-- Email Input -->
      <div class="space-y-1.5">
        <label class="text-xs font-semibold uppercase tracking-wider text-indigo-200" for="email">Email Address</label>
        <!-- 3. FIX: bind:value={email} lagaya -->
        <input type="email" id="email" bind:value={email} placeholder="you@example.com" required class="w-full px-4 py-3 bg-white/5 border border-white/10 rounded-xl text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-200">
      </div>

      <!-- Password Input -->
      <div class="space-y-1.5">
        <label class="text-xs font-semibold uppercase tracking-wider text-indigo-200" for="password">Password</label>
        <!-- 3. FIX: bind:value={password} lagaya -->
        <input type="password" id="password" bind:value={password} placeholder="••••••••" required class="w-full px-4 py-3 bg-white/5 border border-white/10 rounded-xl text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent transition-all duration-200">
      </div>

      <!-- Terms & Conditions Checkbox -->
      <div class="flex items-start gap-3 pt-1">
        <input type="checkbox" id="terms" required class="mt-1 h-4 w-4 rounded border-white/10 bg-white/5 text-indigo-600 focus:ring-indigo-600 focus:ring-offset-slate-950 cursor-pointer">
        <label for="terms" class="text-xs text-indigo-200 leading-normal cursor-pointer select-none">
          I agree to the <a href="/read" class="text-indigo-400 hover:text-indigo-300 underline">Terms of Service</a> and <a href="/privacy" class="text-indigo-400 hover:text-indigo-300 underline">Privacy Policy</a>.
        </label>
      </div>

      <!-- Social Register Buttons -->
  <!-- grid-cols-1 stacks them on mobile; sm:grid-cols-2 splits them on tablet/desktop -->
<div class="grid grid-cols-1 sm:grid-cols-2 gap-3 w-full">
  
  <!-- Google OAuth Button -->
  <button type="button"
  
	onclick={handleGoogleSignIn}
   class="flex items-center justify-center gap-2 px-4 py-3 border border-white/10 rounded-xl cursor-pointer hover:bg-white/5 text-sm w-full min-h-[44px]">
    <svg class="w-4 h-4 shrink-0" xmlns="http://w3.org" viewBox="0 0 48 48">
      <path fill="#EA4335" d="M24 9.5c3.54 0 6.71 1.22 9.21 3.6l6.85-6.85C35.9 2.38 30.47 0 24 0 14.62 0 6.51 5.38 2.56 13.22l7.98 6.19C12.43 13.72 17.74 9.5 24 9.5z"/>
      <path fill="#FBBC05" d="M2.56 13.22A23.88 23.88 0 0 0 0 24c0 3.84.9 7.47 2.49 10.71l7.95-6.17c-.42-1.46-.64-2.99-.64-4.54 0-1.57.23-3.12.66-4.59l-7.9-6.19z"/>
      <path fill="#34A853" d="M24 38.5c-6.26 0-11.57-4.22-13.46-9.96l-7.95 6.17C6.51 42.62 14.62 48 24 48c6.15 0 11.41-2.02 15.24-5.52l-7.4-5.74c-2.11 1.4-4.79 2.26-7.84 2.26z"/>
      <path fill="#4285F4" d="M46.5 24c0-1.63-.15-3.21-.42-4.75H24v9h12.75c-.55 2.96-2.22 5.47-4.75 7.16l7.4 5.74C43.75 37.12 46.5 31.17 46.5 24z"/>
    </svg>
    <span class="font-medium">Continue with Google</span>
  </button>

 <!-- Second Button (e.g., Apple) -->

  <button type="button" onclick={() => alert('Apple Sign-In not implemented yet')}
  class="flex items-center justify-center gap-2 px-4 py-3 border border-white/10 rounded-xl cursor-pointer hover:bg-white/5 text-sm w-full min-h-[44px]">
    <!-- Apple SVG Icon -->
    <svg class="w-4 h-4 shrink-0" fill="currentColor" viewBox="0 0 24 24" xmlns="http://w3.org">
      <path d="M18.71 19.5c-.83 1.24-1.71 2.45-3.05 2.47-1.34.03-1.77-.79-3.29-.79-1.53 0-2 .77-3.27.82-1.31.05-2.3-1.32-3.14-2.53C4.25 17 2.94 12.45 4.7 9.39c.87-1.52 2.43-2.48 4.12-2.51 1.28-.02 2.5.87 3.29.87.78 0 2.26-1.07 3.81-.91.65.03 2.47.26 3.64 1.98-.09.06-2.17 1.28-2.15 3.81.03 3.02 2.65 4.03 2.68 4.04-.03.07-.42 1.44-1.38 2.83M15.97 4.17c.66-.81 1.11-1.93.99-3.06-.96.04-2.13.64-2.82 1.45-.6.7-1.13 1.84-1.01 2.96 1.07.08 2.18-.54 2.84-1.35Z"/>
    </svg>
    <span class="font-medium">Continue with Apple</span>
  </button>

</div>



      <!-- Submit Button -->
      <button type="submit" disabled={loading} class="w-full mt-2 bg-gradient-to-r from-indigo-500 to-purple-600 hover:from-indigo-600 hover:to-purple-700 text-white font-semibold py-3 px-4 rounded-xl shadow-lg shadow-indigo-500/20 transform active:scale-[0.98] transition-all duration-150 cursor-pointer disabled:opacity-50">
        {loading ? 'Registering...' : 'Get Started'}
      </button>
    </form>

  </div>
</div>
