<script lang="ts">
	import { getLocale, setLocale } from '$lib/paraglide/runtime';
	import * as m from '$lib/paraglide/messages';
	import { browser } from '$app/environment';

	// Weekly special dish data
	interface WeeklySpecial {
		image: string;
		title: string;
		subtitle: string;
		description: string;
		lunchPrice: string;
		dinnerPrice: string;
		ingredients: string[];
		orderDeadline: string;
		deliveryDay: string;
		weekOf: string;
	}

	const weeklySpecial: WeeklySpecial = {
		image:
			'https://images.unsplash.com/photo-1512058564366-18510be2db19?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2000&q=80',
		title: m.dish_title(),
		subtitle: m.dish_subtitle(),
		description: m.dish_description(),
		lunchPrice: m.lunch_price(),
		dinnerPrice: m.dinner_price(),
		ingredients: [
			m.ingredients_jasmine_rice(),
			m.ingredients_chicken(),
			m.ingredients_sambal(),
			m.ingredients_pickles(),
			m.ingredients_egg(),
			m.ingredients_shallots()
		],
		orderDeadline: m.order_deadline(),
		deliveryDay: m.delivery_day(),
		weekOf: m.week_of()
	};

	// Language switching functionality
	function switchLanguage(locale: 'en' | 'id') {
		if (browser) {
			setLocale(locale);
		}
	}

	// Handle language change event
	function handleLanguageChange(event: Event) {
		const target = event.currentTarget as HTMLSelectElement;
		const locale = target.value as 'en' | 'id';
		switchLanguage(locale);
	}

	// Get current locale for display
	$: currentLocale = browser ? getLocale() : 'en';

	// Generate WhatsApp message URL
	$: whatsappMessage = m.whatsapp_message({
		title: weeklySpecial.title,
		lunchPrice: weeklySpecial.lunchPrice,
		dinnerPrice: weeklySpecial.dinnerPrice
	});
	$: whatsappUrl = `https://wa.me/6281234567890?text=${whatsappMessage}`;
</script>

<svelte:head>
	<title>{m.page_title()}</title>
	<meta charset="UTF-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0" />
	<meta name="description" content={m.page_description()} />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap"
		rel="stylesheet"
	/>
	<style>
		:global(body) {
			font-family: 'Inter', sans-serif;
		}
	</style>
</svelte:head>

<main class="min-h-screen bg-white">
	<!-- Header -->
	<header class="sticky top-0 z-10 border-b border-gray-100 bg-white/95 backdrop-blur-sm">
		<div class="container mx-auto px-4 py-3">
			<!-- Main header row -->
			<div class="flex items-center justify-between">
				<div class="flex items-center space-x-2 sm:space-x-3">
					<img src="/favicon.png" alt={m.logo_alt()} class="h-10 w-auto sm:h-10" />
					<div>
						<h1 class="text-base font-bold text-gray-900 sm:text-lg">{m.brand_name()}</h1>
						<p class="hidden text-xs text-gray-600 sm:block sm:text-sm">{m.kitchen_tagline()}</p>
					</div>
				</div>

				<div class="flex items-center space-x-2">
					<!-- Week Badge - hidden on mobile, shown on larger screens -->
					<div
						class="hidden rounded-full bg-teal-100 px-3 py-1 text-xs font-medium text-teal-700 sm:block sm:text-sm"
					>
						{m.week_badge()}
					</div>

					<!-- Language Selector - compact on mobile -->
					<div class="relative">
						<select
							on:change={handleLanguageChange}
							value={currentLocale}
							class="appearance-none rounded-lg border border-gray-200 bg-white py-1.5 pr-7 pl-2 text-xs font-medium text-gray-700 hover:border-teal-300 focus:border-teal-500 focus:ring-2 focus:ring-teal-200 focus:outline-none sm:py-2 sm:pr-8 sm:pl-3 sm:text-sm"
						>
							<option value="en">{m.language_en()}</option>
							<option value="id">{m.language_id()}</option>
						</select>

						<!-- Custom dropdown arrow -->
						<div
							class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-1.5 text-gray-700 sm:px-2"
						>
							<svg
								class="h-3 w-3 sm:h-4 sm:w-4"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M19 9l-7 7-7-7"
								/>
							</svg>
						</div>
					</div>
				</div>
			</div>

			<!-- Mobile week badge - shown below main header on mobile only -->
			<div class="mt-2 sm:hidden">
				<div class="text-center">
					<span
						class="inline-block rounded-full bg-teal-100 px-3 py-1 text-xs font-medium text-teal-700"
					>
						{m.week_badge()}
					</span>
				</div>
			</div>
		</div>
	</header>

	<!-- Hero Section -->
	<section class="px-4 py-16">
		<div class="container mx-auto max-w-4xl">
			<!-- Badge -->
			<div class="mb-8 text-center">
				<span
					class="inline-flex items-center rounded-full bg-teal-50 px-4 py-2 text-sm font-medium text-teal-700"
				>
					{m.this_weeks_fusion()}
				</span>
			</div>

			<!-- Title -->
			<div class="mb-12 text-center">
				<h2 class="mb-4 text-3xl font-bold text-gray-900 sm:text-4xl lg:text-5xl">
					{m.dish_title()}
				</h2>
				<p class="mb-4 text-lg font-medium text-teal-600 sm:text-xl">
					{m.dish_subtitle()}
				</p>
				<p class="mx-auto max-w-2xl leading-relaxed text-gray-600 sm:text-lg">
					{m.dish_description()}
				</p>
			</div>

			<div class="mb-12 text-center">
				<img
					src={weeklySpecial.image}
					alt={weeklySpecial.title}
					class="mx-auto w-full max-w-lg rounded-2xl shadow-lg"
				/>
			</div>

			<div class="mb-12 grid grid-cols-1 gap-4 sm:grid-cols-2 sm:gap-6">
				<div class="rounded-xl border border-gray-200 bg-gray-50 p-6 text-center">
					<div class="mb-2 text-sm font-medium text-gray-600">{m.lunch_to_door()}</div>
					<div class="mb-1 text-2xl font-bold text-gray-900">{weeklySpecial.lunchPrice}</div>
					<div class="text-sm text-gray-500">{m.lunch_time_range()}</div>
				</div>
				<div class="rounded-xl border border-gray-200 bg-gray-50 p-6 text-center">
					<div class="mb-2 text-sm font-medium text-gray-600">{m.dinner_at_home()}</div>
					<div class="mb-1 text-2xl font-bold text-gray-900">{weeklySpecial.dinnerPrice}</div>
					<div class="text-sm text-gray-500">{m.dinner_time_range()}</div>
				</div>
			</div>

			<!-- Order Deadline -->
			<div class="mb-12 text-center">
				<div class="mx-auto max-w-sm rounded-xl border border-teal-200 bg-teal-50 p-6">
					<div class="mb-2 text-lg font-semibold text-gray-900">{m.limited_orders()}</div>
					<div class="text-xl font-bold text-teal-700">{weeklySpecial.orderDeadline}</div>
					<div class="mt-2 text-sm text-gray-600">{m.premium_ingredients_note()}</div>
				</div>
			</div>

			<!-- Ingredients -->
			<div class="text-center">
				<h3 class="mb-8 text-2xl font-bold text-gray-900">{m.ingredients_heading()}</h3>
				<div class="mx-auto max-w-md space-y-3">
					{#each weeklySpecial.ingredients as ingredient}
						<div class="flex items-center justify-center">
							<div class="mr-3 h-2 w-2 rounded-full bg-teal-500"></div>
							<span class="text-gray-700">{ingredient}</span>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</section>

	<!-- How It Works -->
	<section class="bg-gradient-to-b from-gray-50 to-white px-4 py-16">
		<div class="container mx-auto max-w-5xl">
			<div class="mb-12 text-center">
				<h3 class="mb-4 text-3xl font-bold text-gray-900">{m.how_it_works_title()}</h3>
				<p class="text-lg text-gray-600">
					{m.how_it_works_subtitle()}
				</p>
			</div>

			<div class="grid grid-cols-1 gap-12 sm:grid-cols-2 lg:grid-cols-4">
				<!-- Step 1 -->
				<div class="group relative text-center">
					<!-- Connection line (hidden on mobile) -->
					<div
						class="absolute top-8 left-full hidden h-0.5 w-full bg-gradient-to-r from-teal-200 to-transparent lg:block"
					></div>

					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl border border-teal-200 bg-white shadow-lg transition-all group-hover:scale-110 group-hover:shadow-xl"
					>
						<span class="text-3xl">📅</span>
					</div>

					<h4 class="mb-3 text-lg font-bold text-gray-900">{m.step1_title()}</h4>
					<p class="leading-relaxed text-gray-600">
						{m.step1_description()}
					</p>
				</div>

				<!-- Step 2 -->
				<div class="group relative text-center">
					<!-- Connection line (hidden on mobile) -->
					<div
						class="absolute top-8 left-full hidden h-0.5 w-full bg-gradient-to-r from-teal-200 to-transparent lg:block"
					></div>

					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl border border-teal-200 bg-white shadow-lg transition-all group-hover:scale-110 group-hover:shadow-xl"
					>
						<span class="text-3xl">🛒</span>
					</div>

					<h4 class="mb-3 text-lg font-bold text-gray-900">{m.step2_title()}</h4>
					<p class="leading-relaxed text-gray-600">
						{m.step2_description()}
					</p>
				</div>

				<!-- Step 3 -->
				<div class="group relative text-center">
					<!-- Connection line (hidden on mobile) -->
					<div
						class="absolute top-8 left-full hidden h-0.5 w-full bg-gradient-to-r from-teal-200 to-transparent lg:block"
					></div>

					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl border border-teal-200 bg-white shadow-lg transition-all group-hover:scale-110 group-hover:shadow-xl"
					>
						<span class="text-3xl">👩‍🍳</span>
					</div>

					<h4 class="mb-3 text-lg font-bold text-gray-900">{m.step3_title()}</h4>
					<p class="leading-relaxed text-gray-600">
						{m.step3_description()}
					</p>
				</div>

				<!-- Step 4 -->
				<div class="group relative text-center">
					<div
						class="mx-auto mb-6 flex h-20 w-20 items-center justify-center rounded-2xl border border-teal-200 bg-white shadow-lg transition-all group-hover:scale-110 group-hover:shadow-xl"
					>
						<span class="text-3xl">🚚</span>
					</div>

					<h4 class="mb-3 text-lg font-bold text-gray-900">{m.step4_title()}</h4>
					<p class="leading-relaxed text-gray-600">
						{m.step4_description()}
					</p>
				</div>
			</div>

			<!-- Bottom feature highlight -->
			<div class="mt-12 text-center">
				<div
					class="inline-flex items-center rounded-full bg-teal-50 px-6 py-3 text-sm font-medium text-teal-700"
				>
					<span class="mr-2">🌍</span>
					{m.fusion_tagline()}
				</div>
			</div>
		</div>
	</section>

	<!-- Order Now Section -->
	<section class="px-4 py-16">
		<div class="container mx-auto max-w-4xl">
			<div class="mb-12 text-center">
				<h3 class="mb-4 text-3xl font-bold text-gray-900">{m.order_cta_title()}</h3>
				<p class="text-lg text-gray-600">{m.order_cta_subtitle()}</p>
			</div>

			<div class="grid grid-cols-1 gap-6 lg:grid-cols-2">
				<!-- WhatsApp Order -->
				<a
					href={whatsappUrl}
					class="group flex flex-col rounded-2xl border border-gray-200 bg-white p-6 transition-all hover:border-teal-200 hover:shadow-lg active:scale-95"
				>
					<div class="mb-4 flex items-center">
						<div class="mr-4 flex h-12 w-12 items-center justify-center rounded-full bg-green-100">
							<span class="text-xl">💬</span>
						</div>
						<div>
							<h4 class="text-lg font-bold text-gray-900">{m.whatsapp_title()}</h4>
							<p class="text-sm text-gray-600">{m.whatsapp_subtitle()}</p>
						</div>
					</div>

					<div class="mb-6 flex-grow space-y-2">
						<div class="flex justify-between text-sm">
							<span class="text-gray-600">{m.lunch_time()}</span>
							<span class="font-semibold text-gray-900">{weeklySpecial.lunchPrice}</span>
						</div>
						<div class="flex justify-between text-sm">
							<span class="text-gray-600">{m.dinner_time()}</span>
							<span class="font-semibold text-gray-900">{weeklySpecial.dinnerPrice}</span>
						</div>
					</div>

					<div class="mt-auto">
						<span
							class="inline-flex w-full items-center justify-center rounded-lg bg-teal-100 px-4 py-3 text-sm font-medium text-teal-700 group-hover:bg-teal-200"
						>
							{m.whatsapp_button()}
						</span>
					</div>
				</a>

				<!-- Instagram Order -->
				<a
					href="https://instagram.com/keykey.kitchen"
					class="group flex flex-col rounded-2xl border border-gray-200 bg-white p-6 transition-all hover:border-teal-200 hover:shadow-lg active:scale-95"
				>
					<div class="mb-4 flex items-center">
						<div class="mr-4 flex h-12 w-12 items-center justify-center rounded-full bg-pink-100">
							<span class="text-xl">💕</span>
						</div>
						<div>
							<h4 class="text-lg font-bold text-gray-900">{m.instagram_title()}</h4>
							<p class="text-sm text-gray-600">{m.instagram_subtitle()}</p>
						</div>
					</div>

					<div class="mb-6 flex-grow space-y-1 text-sm text-gray-600">
						<p>{m.instagram_feature1()}</p>
						<p>{m.instagram_feature2()}</p>
						<p>{m.instagram_feature3()}</p>
						<p>{m.instagram_feature4()}</p>
					</div>

					<div class="mt-auto">
						<span
							class="inline-flex w-full items-center justify-center rounded-lg bg-teal-100 px-4 py-3 text-sm font-medium text-teal-700 group-hover:bg-teal-200"
						>
							{m.instagram_button()}
						</span>
					</div>
				</a>
			</div>
		</div>
	</section>

	<!-- Footer -->
	<footer class="border-t border-gray-100 px-4 py-12">
		<div class="container mx-auto max-w-4xl text-center">
			<div class="mb-6">
				<img src="/favicon.png" alt={m.logo_alt()} class="mx-auto h-10 w-auto" />
			</div>
			<h4 class="mb-2 text-xl font-bold text-gray-900">{m.brand_name()}</h4>
			<p class="mb-4 text-gray-600">
				{m.footer_description()}
			</p>
			<p class="text-sm text-gray-500">
				{m.footer_tagline()}
			</p>
		</div>
	</footer>
</main>
