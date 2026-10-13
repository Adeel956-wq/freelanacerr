<script lang="ts">
	// Svelte 5 Runes for tab switching and active section tracking
	let activeTab = $state<'privacy' | 'terms'>('privacy');
	let activeSection = $state('');

	function scrollToSection(id: string) {
		activeSection = id;
		const element = document.getElementById(id);
		if (element) {
			element.scrollIntoView({ behavior: 'smooth', block: 'start' });
		}
	}
</script>

<svelte:head>
	<title>{activeTab === 'privacy' ? 'Privacy Policy' : 'Terms of Service'} | Project Freelancer</title>
</svelte:head>

<div class="min-h-screen bg-slate-50 font-sans text-slate-800 antialiased">
	<!-- Page Header Banner -->
	<header class="border-b border-slate-200/80 bg-slate-900 py-12 text-white">
		<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
			<div class="flex flex-col gap-4 md:flex-row md:items-center md:justify-between">
				<div>
					<span class="inline-flex items-center gap-1.5 rounded-full border border-blue-500/30 bg-blue-500/10 px-3 py-1 text-xs font-semibold text-blue-400">
						🔒 Legal & Compliance Center
					</span>
					<h1 class="mt-3 text-3xl font-black tracking-tight sm:text-4xl">
						{activeTab === 'privacy' ? 'Privacy Policy' : 'Terms of Service'}
					</h1>
					<p class="mt-1 text-sm text-slate-400">
						Last updated: <span class="font-semibold text-slate-200">October 13, 2026</span>
					</p>
				</div>

				<!-- Tab Switcher Controls -->
				<div class="inline-flex rounded-2xl bg-slate-800/80 p-1.5 border border-slate-700/80">
					<button
						onclick={() => (activeTab = 'privacy')}
						class={`rounded-xl px-5 py-2.5 text-sm font-bold transition-all duration-200 ${
							activeTab === 'privacy'
								? 'bg-blue-600 text-white shadow-md'
								: 'text-slate-400 hover:text-white'
						}`}
					>
						Privacy Policy
					</button>
					<button
						onclick={() => (activeTab = 'terms')}
						class={`rounded-xl px-5 py-2.5 text-sm font-bold transition-all duration-200 ${
							activeTab === 'terms'
								? 'bg-blue-600 text-white shadow-md'
								: 'text-slate-400 hover:text-white'
						}`}
					>
						Terms of Service
					</button>
				</div>
			</div>
		</div>
	</header>

	<!-- Main Document Container -->
	<main class="mx-auto max-w-7xl px-4 py-12 sm:px-6 lg:px-8">
		<div class="grid grid-cols-1 gap-10 lg:grid-cols-4">
			
			<!-- Left Sticky Sidebar (Table of Contents) -->
			<aside class="hidden lg:block lg:col-span-1">
				<div class="sticky top-24 rounded-2xl border border-slate-200/80 bg-white p-5 shadow-sm">
					<h3 class="text-xs font-extrabold uppercase tracking-wider text-slate-400">
						Table of Contents
					</h3>

					{#if activeTab === 'privacy'}
						<nav class="mt-4 flex flex-col space-y-1 text-sm font-semibold text-slate-600">
							<button onclick={() => scrollToSection('p-1')} class="text-left py-1.5 hover:text-blue-600 transition">1. Information We Collect</button>
							<button onclick={() => scrollToSection('p-2')} class="text-left py-1.5 hover:text-blue-600 transition">2. How We Use Information</button>
							<button onclick={() => scrollToSection('p-3')} class="text-left py-1.5 hover:text-blue-600 transition">3. Storage & Security</button>
							<button onclick={() => scrollToSection('p-4')} class="text-left py-1.5 hover:text-blue-600 transition">4. Data Sharing</button>
							<button onclick={() => scrollToSection('p-5')} class="text-left py-1.5 hover:text-blue-600 transition">5. Cookies & LocalStorage</button>
							<button onclick={() => scrollToSection('p-6')} class="text-left py-1.5 hover:text-blue-600 transition">6. Your Data Rights</button>
							<button onclick={() => scrollToSection('p-7')} class="text-left py-1.5 hover:text-blue-600 transition">7. Contact Us</button>
						</nav>
					{:else}
						<nav class="mt-4 flex flex-col space-y-1 text-sm font-semibold text-slate-600">
							<button onclick={() => scrollToSection('t-1')} class="text-left py-1.5 hover:text-blue-600 transition">1. Account Eligibility</button>
							<button onclick={() => scrollToSection('t-2')} class="text-left py-1.5 hover:text-blue-600 transition">2. Marketplace Conduct</button>
							<button onclick={() => scrollToSection('t-3')} class="text-left py-1.5 hover:text-blue-600 transition">3. Verification & Security</button>
							<button onclick={() => scrollToSection('t-4')} class="text-left py-1.5 hover:text-blue-600 transition">4. Intellectual Property</button>
							<button onclick={() => scrollToSection('t-5')} class="text-left py-1.5 hover:text-blue-600 transition">5. Fees & Payments</button>
							<button onclick={() => scrollToSection('t-6')} class="text-left py-1.5 hover:text-blue-600 transition">6. Limitation of Liability</button>
							<button onclick={() => scrollToSection('t-7')} class="text-left py-1.5 hover:text-blue-600 transition">7. Termination</button>
						</nav>
					{/if}

					<div class="mt-6 border-t border-slate-100 pt-4">
						<p class="text-xs text-slate-400">Need help regarding legal terms?</p>
						<a href="mailto:privacy@projectfreelancer.com" class="mt-1 block text-xs font-bold text-blue-600 hover:underline">
							Contact Legal Support &rarr;
						</a>
					</div>
				</div>
			</aside>

			<!-- Right Main Document Content -->
			<div class="lg:col-span-3">
				<div class="rounded-3xl border border-slate-200/80 bg-white p-6 sm:p-10 shadow-sm">
					
					<!-- TAB 1: PRIVACY POLICY -->
					{#if activeTab === 'privacy'}
						<article class="space-y-8 text-slate-700 leading-relaxed">
							<div>
								<h2 class="text-2xl font-black text-slate-900">Privacy Policy</h2>
								<p class="mt-2 text-sm text-slate-600">
									Welcome to <strong>Project Freelancer</strong> ("we," "our," or "us"). We are committed to protecting your privacy and ensuring the security of your personal information. This Privacy Policy explains how we collect, use, disclose, and safeguard your data when you visit our marketplace platform[cite: 1].
								</p>
							</div>

							<hr class="border-slate-100" />

							<!-- Section 1 -->
							<section id="p-1" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">1. Information We Collect</h3>
								<p class="text-sm text-slate-600">
									We collect data to provide better services to all users, verify user identities, prevent fraud, and maintain platform security[cite: 1, 3]:
								</p>
								<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 mt-2">
									<div class="rounded-xl border border-slate-200 bg-slate-50/50 p-4">
										<h4 class="text-xs font-bold uppercase tracking-wider text-blue-600">Provided Information</h4>
										<ul class="mt-2 text-xs space-y-1.5 text-slate-600 list-disc list-inside">
											<li>Full Name & Email Address[cite: 1]</li>
											<li>Hashed passwords (encrypted with bcrypt)[cite: 1, 3]</li>
											<li>OAuth Provider IDs (Google/GitHub)[cite: 1, 3]</li>
											<li>Skills, hourly rates, and profile bio[cite: 1]</li>
										</ul>
									</div>
									<div class="rounded-xl border border-slate-200 bg-slate-50/50 p-4">
										<h4 class="text-xs font-bold uppercase tracking-wider text-blue-600">Automated Technical Metadata</h4>
										<ul class="mt-2 text-xs space-y-1.5 text-slate-600 list-disc list-inside">
											<li>IP Address (X-Forwarded-For headers)[cite: 2, 3]</li>
											<li>Device User-Agent & Browser details[cite: 1, 2, 3]</li>
											<li>Screen resolution (e.g., 1920x1080)[cite: 1, 2, 3]</li>
											<li>Security action logs & navigation URLs[cite: 1, 2, 3]</li>
										</ul>
									</div>
								</div>
							</section>

							<!-- Section 2 -->
							<section id="p-2" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">2. How We Use Your Information</h3>
								<ul class="space-y-2 text-sm text-slate-600 list-disc list-inside">
									<li><strong>Service Provision:</strong> Facilitating client-freelancer connections and handling project orders[cite: 1].</li>
									<li><strong>Platform Security:</strong> Verifying email accounts, tracking suspicious activity, and preventing unauthorized access[cite: 1, 3].</li>
									<li><strong>Performance Optimization:</strong> Analyzing display resolutions and network metrics to refine UI layouts.</li>
								</ul>
							</section>

							<!-- Section 3 -->
							<section id="p-3" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">3. Storage & Security Infrastructure</h3>
								<p class="text-sm text-slate-600">
									Passwords are encrypted using bcrypt algorithms[cite: 1, 3]. Portfolio images, media assets, and uploaded documents are stored securely using <strong>Cloudflare R2 Object Storage</strong> with restricted bucket access policies[cite: 1, 2]. Core data is stored securely in structured database systems with regular backup protocols[cite: 1, 2, 3].
								</p>
							</section>

							<!-- Section 4 -->
							<section id="p-4" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">4. Disclosure & Data Sharing</h3>
								<p class="text-sm text-slate-600">
									We do not sell, rent, or trade user personal data to third parties. Data is shared strictly with infrastructure providers (hosting, Cloudflare R2, mail delivery services) or when required by legal authorities[cite: 1, 2, 3].
								</p>
							</section>

							<!-- Section 5 -->
							<section id="p-5" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">5. Cookies & Local Storage</h3>
								<p class="text-sm text-slate-600">
									We use browser <code class="rounded bg-slate-100 px-1.5 py-0.5 text-xs text-blue-700">localStorage</code> and HTTP session tokens (<code class="rounded bg-slate-100 px-1.5 py-0.5 text-xs text-blue-700">access_token</code>) to maintain authentication sessions and user preference configurations[cite: 1, 2, 3].
								</p>
							</section>

							<!-- Section 6 -->
							<section id="p-6" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">6. Your Data Rights</h3>
								<p class="text-sm text-slate-600">
									You have the right to request access to your personal data, request corrections, or request complete account deletion (subject to ongoing active contract resolutions)[cite: 1].
								</p>
							</section>

							<!-- Section 7 -->
							<section id="p-7" class="scroll-mt-24 space-y-3 pt-4 border-t border-slate-100">
								<h3 class="text-lg font-bold text-slate-900">7. Contact Us</h3>
								<p class="text-sm text-slate-600">
									If you have any questions regarding this policy, please reach out:
								</p>
								<div class="mt-2 rounded-xl bg-slate-50 p-4 border border-slate-200">
									<p class="text-xs font-bold text-slate-900">Project Freelancer Legal Team</p>
									<p class="text-xs text-slate-500 mt-0.5">Email: privacy@projectfreelancer.com</p>
								</div>
							</section>
						</article>

					<!-- TAB 2: TERMS OF SERVICE -->
					{:else}
						<article class="space-y-8 text-slate-700 leading-relaxed">
							<div>
								<h2 class="text-2xl font-black text-slate-900">Terms of Service</h2>
								<p class="mt-2 text-sm text-slate-600">
									By accessing or using <strong>Project Freelancer</strong>, you agree to be legally bound by these Terms of Service. Please review them carefully[cite: 1].
								</p>
							</div>

							<hr class="border-slate-100" />

							<!-- Section 1 -->
							<section id="t-1" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">1. Account Eligibility & Rules</h3>
								<ul class="space-y-2 text-sm text-slate-600 list-disc list-inside">
									<li>You must be at least 18 years old or legal majority age in your jurisdiction.</li>
									<li>You must provide accurate profile details during registration[cite: 1, 3].</li>
									<li>Users are prohibited from creating fake duplicate accounts to manipulate ratings or platform bidding[cite: 1].</li>
								</ul>
							</section>

							<!-- Section 2 -->
							<section id="t-2" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">2. User Roles & Marketplace Conduct</h3>
								<p class="text-sm text-slate-600">
									<strong>Freelancers:</strong> Must accurately represent skills, meet agreed deadlines, and deliver quality service work[cite: 1].<br />
									<strong>Clients:</strong> Agree to provide clear project requirements and process payments for approved work.
								</p>
								<div class="rounded-xl border border-red-100 bg-red-50/50 p-4">
									<h4 class="text-xs font-bold text-red-700 uppercase tracking-wider">Prohibited Actions</h4>
									<p class="mt-1 text-xs text-red-600">
										Probing system vulnerabilities, automated scraping, reverse engineering backend APIs, or processing off-platform payments to evade platform fees[cite: 1, 2].
									</p>
								</div>
							</section>

							<!-- Section 3 -->
							<section id="t-3" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">3. Identity Verification & Security</h3>
								<p class="text-sm text-slate-600">
									Project Freelancer reserves the right to log connection technical metrics (IP address, device User-Agent) and perform KYC verification checks to safeguard marketplace transactions[cite: 1, 2, 3].
								</p>
							</section>

							<!-- Section 4 -->
							<section id="t-4" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">4. Intellectual Property Rights</h3>
								<p class="text-sm text-slate-600">
									All custom work intellectual property rights transfer to the client upon full payment release. All platform assets, logos, and software belong exclusively to Project Freelancer[cite: 4].
								</p>
							</section>

							<!-- Section 5 -->
							<section id="t-5" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">5. Fees & Payments</h3>
								<p class="text-sm text-slate-600">
									Platform service fees are displayed prior to transaction confirmation. Users are responsible for local tax liabilities associated with earnings or payouts.
								</p>
							</section>

							<!-- Section 6 -->
							<section id="t-6" class="scroll-mt-24 space-y-3">
								<h3 class="text-lg font-bold text-slate-900">6. Limitation of Liability</h3>
								<p class="text-sm text-slate-600">
									Project Freelancer shall not be held liable for indirect, incidental, or consequential damages resulting from platform service disruptions or third-party interactions.
								</p>
							</section>

							<!-- Section 7 -->
							<section id="t-7" class="scroll-mt-24 space-y-3 pt-4 border-t border-slate-100">
								<h3 class="text-lg font-bold text-slate-900">7. Termination & Inquiries</h3>
								<p class="text-sm text-slate-600">
									Accounts violating marketplace policies may be suspended or terminated. For inquiries regarding terms, contact:
								</p>
								<div class="mt-2 rounded-xl bg-slate-50 p-4 border border-slate-200">
									<p class="text-xs font-bold text-slate-900">Project Freelancer Support</p>
									<p class="text-xs text-slate-500 mt-0.5">Email: support@projectfreelancer.com</p>
								</div>
							</section>
						</article>
					{/if}

				</div>
			</div>

		</div>
	</main>
</div>