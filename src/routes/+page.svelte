<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import OldLogo from '$lib/old-logo.svelte';
	import MOneLogo from '$lib/m-one-logo.svelte';
	
	let mounted = false;
	let showContent = false;
	
	onMount(() => {
		// Prevent FOUC by showing content after mount
		setTimeout(() => {
			mounted = true;
			showContent = true;
		}, 100);
	});
	
	function handleClick() {
		window.open('https://m-onecapital.com', '_blank');
	}
	
	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter' || event.key === ' ') {
			event.preventDefault();
			handleClick();
		}
	}
</script>

<svelte:head>
	<title>McCarthy Capital is now M-One Capital</title>
	<meta name="description" content="McCarthy Capital has transitioned to M-One Capital. Visit our new website to learn more about our continued investment focus." />
	<meta name="viewport" content="width=device-width, initial-scale=1.0" />
	<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
	<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
	<link rel="icon" type="image/png" href="/favicon.png" />
	<link rel="apple-touch-icon" href="/apple-favicon.png" />
</svelte:head>

<!-- Loading overlay -->
{#if !showContent}
	<div class="loading-overlay" aria-label="Loading page content">
		<div class="loading-spinner" aria-hidden="true"></div>
		<span class="sr-only">Loading...</span>
	</div>
{/if}

{#if showContent}
	<main class="container" in:fade={{ duration: 800 }}>
		<!-- Screen reader announcement -->
		<div class="sr-only" aria-live="polite">
			McCarthy Capital has transitioned to M-One Capital. Click anywhere to visit the new website.
		</div>
		
		<div class="content-wrapper" 
			 role="button" 
			 tabindex="0"
			 onclick={handleClick} 
			 onkeydown={handleKeydown}
			 aria-label="Click to visit M-One Capital website">
			
			<!-- McCarthy Logo (Top) -->
			<div class="logo-section" in:fade={{ duration: 1000, delay: 200 }}>
				<div class="logo-wrapper old-logo" aria-label="McCarthy Capital logo">
					<OldLogo />
				</div>
			</div>
			
			<!-- Transition Text -->
			<div class="transition-section" in:fade={{ duration: 1000, delay: 1200 }}>
				<h1 class="main-heading">
					<span class="transition-text" aria-label="is now">is now</span>
				</h1>
			</div>
			
			<!-- M-One Logo (Bottom) -->
			<div class="logo-section" in:fade={{ duration: 1000, delay: 2000 }}>
				<div class="logo-wrapper new-logo" aria-label="M-One Capital logo">
					<MOneLogo />
				</div>
			</div>
			
			<!-- Call to Action -->
			<div class="cta-section" in:fly={{ y: 30, duration: 600, delay: 2800 }}>
				<button 
					class="visit-button" 
					onclick={handleClick}
					aria-label="Visit M-One Capital website">
					<span>Visit Our New Site</span>
					<span class="arrow" aria-hidden="true">→</span>
				</button>
			</div>
		</div>
		
		<!-- WordPress Export Notice (hidden by default) -->
		<div id="wordpress-export-styles" style="display: none;">
			<!-- Styles will be extracted for WordPress -->
		</div>
	</main>
{/if}

<style>
	@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600;700&display=swap');
	@import url('https://fonts.adobe.com/fonts/adobe-caslon');
	
	/* Reset and base styles */
	* {
		box-sizing: border-box;
	}
	
	:global(body) {
		margin: 0;
		padding: 0;
		font-family: 'Playfair Display', serif;
		background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
		min-height: 100vh;
	}
	
	/* Screen reader only content */
	.sr-only {
		position: absolute;
		width: 1px;
		height: 1px;
		padding: 0;
		margin: -1px;
		overflow: hidden;
		clip: rect(0, 0, 0, 0);
		white-space: nowrap;
		border: 0;
	}
	
	/* Loading overlay */
	.loading-overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		z-index: 9999;
	}
	
	.loading-spinner {
		width: 40px;
		height: 40px;
		border: 3px solid rgba(108, 141, 200, 0.3);
		border-top: 3px solid rgb(108, 141, 200);
		border-radius: 50%;
		animation: spin 1s linear infinite;
		margin-bottom: 1rem;
	}
	
	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}
	
	/* Main container */
	.container {
		min-height: 100vh;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 2rem;
	}
	
	.content-wrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		max-width: 1200px;
		width: 100%;
		cursor: pointer;
		outline: none;
		border-radius: 12px;
		padding: 2rem;
		transition: all 0.3s ease;
	}
	
	.content-wrapper:focus {
		outline: 3px solid rgb(108, 141, 200);
		outline-offset: 4px;
	}
	
	.content-wrapper:hover {
		transform: translateY(-2px);
	}
	
	/* Logo sections */
	.logo-section {
		width: 100%;
		display: flex;
		justify-content: center;
		margin: 1.5rem 0;
	}
	
	.logo-wrapper {
		width: 200px;
		height: 100px;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 0.3s ease;
		filter: drop-shadow(0 8px 32px rgba(108, 141, 200, 0.15));
	}
	
	.logo-wrapper:hover {
		transform: scale(1.05);
	}
	
	.logo-wrapper :global(svg) {
		width: 100%;
		height: 100%;
		max-width: 100%;
		max-height: 100%;
		object-fit: contain;
	}
	
	/* Transition section */
	.transition-section {
		margin: 2rem 0;
		text-align: center;
	}
	
	.main-heading {
		margin: 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 0.5rem;
	}
	
	.company-name {
		font-family: adobe-caslon-pro, serif;
		font-size: 1.6rem;
		font-weight: 400;
		color: rgb(108, 141, 200);
		text-shadow: 0 2px 4px rgba(108, 141, 200, 0.1);
		line-height: 1.1;
		transition: font-size 0.3s ease;
	}
	
	.transition-text {
		font-family: 'Playfair Display', serif;
		font-size: 1rem;
		font-weight: 300;
		color: rgba(108, 141, 200, 0.7);
		font-style: italic;
		margin: 0.5rem 0;
		transition: font-size 0.3s ease;
	}
	
	/* Call to action */
	.cta-section {
		margin-top: 2rem;
	}
	
	.visit-button {
		padding: 1.2rem 3rem;
		background-color: #aca198;
		color: white;
		border: 2px solid #aca198;
		border-radius: 0;
		font-family: 'Playfair Display', serif;
		font-size: 1.4rem;
		font-weight: 600;
		cursor: pointer;
		transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
		display: flex;
		align-items: center;
		gap: 1rem;
		box-shadow: 0 8px 25px rgba(172, 161, 152, 0.3);
		position: relative;
		overflow: hidden;
		text-transform: uppercase;
		letter-spacing: 1px;
	}
	
	.visit-button::before {
		content: '';
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
		transition: left 0.6s;
	}
	
	.visit-button:hover::before {
		left: 100%;
	}
	
	.visit-button:hover {
		background-color: transparent;
		color: #aca198;
		transform: translateY(-3px);
		box-shadow: 0 12px 35px rgba(172, 161, 152, 0.4);
	}
	
	.visit-button:focus {
		outline: 3px solid rgb(108, 141, 200);
		outline-offset: 2px;
	}
	
	.visit-button:hover .arrow {
		transform: translateX(5px);
	}
	
	.arrow {
		font-size: 1.3rem;
		transition: transform 0.3s ease;
	}
	
	/* Responsive design - Mobile First Approach */
	@media (min-width: 321px) {
		.logo-wrapper {
			width: 220px;
			height: 110px;
		}
		
		.company-name {
			font-size: 1.8rem;
		}
		
		.transition-text {
			font-size: 1.1rem;
		}
	}
	
	@media (min-width: 481px) {
		.logo-wrapper {
			width: 280px;
			height: 140px;
		}
		
		.company-name {
			font-size: 2.2rem;
		}
		
		.transition-text {
			font-size: 1.4rem;
		}
		
		.logo-section {
			margin: 1.2rem 0;
		}
		
		.transition-section {
			margin: 1.5rem 0;
		}
		
		.cta-section {
			margin-top: 1.5rem;
		}
	}
	
	@media (min-width: 769px) {
		.container {
			padding: 1rem;
		}
		
		.content-wrapper {
			padding: 1.5rem;
			height: auto;
			justify-content: center;
		}
		
		.logo-wrapper {
			width: 350px;
			height: 175px;
		}
		
		.company-name {
			font-size: 2.8rem;
		}
		
		.transition-text {
			font-size: 1.7rem;
		}
		
		.logo-section {
			margin: 1.5rem 0;
		}
		
		.transition-section {
			margin: 2rem 0;
		}
		
		.cta-section {
			margin-top: 2rem;
		}
	}
	
	@media (min-width: 1025px) {
		.container {
			padding: 2rem;
		}
		
		.content-wrapper {
			padding: 2rem;
		}
		
		.logo-wrapper {
			width: 400px;
			height: 200px;
		}
		
		.company-name {
			font-size: 3.5rem;
		}
		
		.transition-text {
			font-size: 2rem;
		}
		
		.logo-section {
			margin: 1.5rem 0;
		}
		
		.transition-section {
			margin: 2rem 0;
		}
		
		.cta-section {
			margin-top: 2rem;
		}
	}
	
	/* Mobile-specific adjustments */
	@media (max-width: 480px) {
		.container {
			padding: 0.3rem;
			min-height: 100vh;
		}
		
		.content-wrapper {
			padding: 0.5rem;
			height: 100vh;
			justify-content: space-around;
		}
		
		.visit-button {
			font-size: 0.9rem;
			padding: 0.7rem 1.8rem;
		}
		
		.logo-section {
			margin: 0.4rem 0;
		}
		
		.transition-section {
			margin: 0.6rem 0;
		}
		
		.cta-section {
			margin-top: 0.8rem;
		}
	}
	
	@media (max-width: 320px) {
		.logo-wrapper {
			width: 180px;
			height: 90px;
		}
		
		.company-name {
			font-size: 1.4rem;
		}
		
		.transition-text {
			font-size: 0.9rem;
		}
		
		.visit-button {
			font-size: 0.75rem;
			padding: 0.5rem 1.3rem;
		}
	}
	
	/* High contrast mode support */
	@media (prefers-contrast: high) {
		.company-name {
			color: #000;
			text-shadow: none;
		}
		
		.transition-text {
			color: #333;
		}
		
		.visit-button {
			border: 3px solid #000;
		}
	}
	
	/* Reduced motion support */
	@media (prefers-reduced-motion: reduce) {
		* {
			animation-duration: 0.01ms !important;
			animation-iteration-count: 1 !important;
			transition-duration: 0.01ms !important;
		}
		
		.loading-spinner {
			animation: none;
		}
	}
</style>

