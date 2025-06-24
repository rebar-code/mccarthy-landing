<script>
	import { onMount } from 'svelte';
	import { crossfade, fly, fade, draw } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
	import OldLogo from '$lib/old-logo.svelte';
	import MOneLogo from '$lib/m-one-logo.svelte';
	
	let showTransition = false;
	let showClickPrompt = false;
	let mounted = false;
	let pageLoaded = false;
	
	const [send, receive] = crossfade({
		duration: 2000,
		easing: quintOut,
		fallback: (node, params) => {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;
			
			return {
				duration: 2000,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});
	
	onMount(() => {
		// Prevent FOUC by showing content after mount
		setTimeout(() => {
			mounted = true;
			pageLoaded = true;
		}, 100);
		
		// Start the transition after content is loaded
		setTimeout(() => {
			showTransition = true;
			// Show click prompt after transition completes
			setTimeout(() => {
				showClickPrompt = true;
			}, 2500);
		}, 1500);
	});
	
	function handleClick() {
		window.open('https://m-onecapital.com', '_blank');
	}
</script>

<!-- Loading overlay -->
{#if !pageLoaded}
	<div class="loading-overlay">
		<div class="loading-spinner"></div>
	</div>
{/if}

{#if pageLoaded}
	<div class="container" onclick={handleClick} onkeydown={(e) => e.key === 'Enter' && handleClick()} in:fade={{ duration: 800 }}>
		<h1 class="announcement" in:fade={{ duration: 1000, delay: 200 }}>
			McCarthyCapital<br>
			<span class="transition-text">is now</span><br> 
			M-OneCapital
		</h1>
		
		<div class="logo-container" in:fade={{ duration: 1000, delay: 400 }}>
			<!-- McCarthy Capital Logo -->
			{#if !showTransition}
				<div class="logo mccarthy-logo" class:mounted out:send={{key: 'logo'}}>
					<OldLogo />
				</div>
			{/if}
			
			<!-- M-One Capital Logo -->
			{#if showTransition}
				<div class="logo mone-logo" class:mounted in:receive={{key: 'logo'}}>
					<MOneLogo />
				</div>
			{/if}
		</div>
		
		<!-- Click prompt -->
		{#if showClickPrompt}
			<button 
				class="visit-button" 
				onclick={handleClick}
				in:fly={{ y: 30, duration: 600, delay: 200 }}
			>
				<span>Visit M-One Capital</span>
				<div class="arrow">→</div>
			</button>
		{/if}
	</div>
{/if}

<style>
	@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600;700&display=swap');
	@import url('https://fonts.adobe.com/fonts/adobe-caslon');
	
	/* Loading overlay */
	.loading-overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
		display: flex;
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
	}
	
	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}
	
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		padding: 2rem;
		min-height: 100vh;
	}
	
	.logo-container {
		position: relative;
		width: 800px;
		height: 800px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	
	.logo {
		position: absolute;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: all 2s ease-in-out;
		visibility: hidden;
		filter: drop-shadow(0 8px 32px rgba(108, 141, 200, 0.15));
	}
	
	.logo :global(svg) {
		width: 100%;
		height: 100%;
		max-width: 100%;
		max-height: 100%;
		object-fit: contain;
		transition: transform 0.3s ease;
	}
	
	.logo:hover :global(svg) {
		transform: scale(1.02);
	}
	
	/* Show logos only after component mounts */
	.logo.mounted {
		visibility: visible;
	}
	
	.mccarthy-logo,
	.mone-logo {
		opacity: 1;
	}
	
	.announcement {
		box-sizing: border-box;
		clear: both;
		color: rgb(108, 141, 200);
		display: block;
		font-family: adobe-caslon-pro, serif;
		font-size: 52px;
		font-style: normal;
		font-weight: 400;
		text-align: center;
		margin-bottom: 3rem;
		line-height: 1.2;
		text-shadow: 0 2px 4px rgba(108, 141, 200, 0.1);
	}
	
	.transition-text {
		font-size: 32px;
		font-weight: 300;
		color: rgba(108, 141, 200, 0.8);
		font-style: italic;
	}
	
	.visit-button {
		margin-top: 4rem;
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
	
	.visit-button:hover .arrow {
		transform: translateX(5px);
	}
	
	.arrow {
		font-size: 1.3rem;
		transition: transform 0.3s ease;
		animation: pulse 2s infinite;
	}
	
	@keyframes pulse {
		0%, 100% {
			opacity: 1;
		}
		50% {
			opacity: 0.7;
		}
	}
	
	/* Responsive design */
	@media (max-width: 1024px) {
		.logo-container {
			width: 600px;
			height: 600px;
		}
	}
	
	@media (max-width: 768px) {
		.announcement {
			font-size: 36px;
			margin-bottom: 2rem;
		}
		
		.transition-text {
			font-size: 24px;
		}
		
		.logo-container {
			width: 500px;
			height: 500px;
		}
		
		.container {
			padding: 1rem;
		}
		
		.visit-button {
			font-size: 1.2rem;
			padding: 1rem 2.5rem;
			margin-top: 3rem;
		}
	}
	
	@media (max-width: 640px) {
		.logo-container {
			width: 400px;
			height: 400px;
		}
		
		.announcement {
			font-size: 32px;
		}
		
		.transition-text {
			font-size: 22px;
		}
	}
	
	@media (max-width: 480px) {
		.announcement {
			font-size: 28px;
			margin-bottom: 1.5rem;
		}
		
		.transition-text {
			font-size: 20px;
		}
		
		.logo-container {
			width: 320px;
			height: 320px;
		}
		
		.visit-button {
			font-size: 1rem;
			padding: 0.9rem 2rem;
			margin-top: 2.5rem;
		}
	}
	
	@media (max-width: 360px) {
		.logo-container {
			width: 280px;
			height: 280px;
		}
		
		.announcement {
			font-size: 24px;
		}
		
		.transition-text {
			font-size: 18px;
		}
		
		.container {
			padding: 0.5rem;
		}
		
		.visit-button {
			font-size: 0.9rem;
			padding: 0.8rem 1.8rem;
		}
	}
</style>
