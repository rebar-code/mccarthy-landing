<script lang="ts">
	import '../app.css';

	let { children } = $props();
</script>
<div class="wrapper">
{@render children()}
</div>

<style> 

.wrapper {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		min-height: 100vh;
		background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
		cursor: pointer;
		padding: 2rem;
		font-family: 'Playfair Display', serif;
	}
</style>
