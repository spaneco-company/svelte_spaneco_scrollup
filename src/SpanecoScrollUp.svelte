<script lang="ts">
	import { onMount } from 'svelte';

	export let showOnPx = 150;
	let hidden = true;

	function scrollContainer() {
		return document.documentElement || document.body;
	}

	function handleOnScroll() {
		if (!scrollContainer()) {
			return;
		}

		if (scrollContainer().scrollTop > showOnPx) {
			hidden = false;
		} else {
			hidden = true;
		}
	}

	function scrollToTop() {
		scrollContainer().scrollTo({
			top: 0,
			behavior: "smooth"
		});
	}

	onMount(() => {
		handleOnScroll();
	});
</script>

<!--Add this line to your web component-->
<svelte:options tag="spaneco-scroll-up" />

<svelte:window on:scroll="{handleOnScroll}" />

<button class="spaneco-scrollup" class:spaneco-scrollup--visible={!hidden} aria-hidden="true" tabindex="-1" on:click={scrollToTop}>
	<div class="spaneco-scrollup__content">
		<svg class="spaneco-scrollup__icon" viewBox="0 0 17 10">
			<path fill="currentColor" d="M14.692 9.63a1.211 1.211 0 101.712-1.715L9.602 1.121a1.211 1.211 0 00-1.714.002L1.113 7.917a1.211 1.211 0 101.716 1.71l5.919-5.935 5.944 5.937z"></path>
		</svg>
	</div>
</button>

<style>
	.spaneco-scrollup {
		position: fixed;
		right: 0;
		bottom: 0;
		z-index: 100;
		padding: 10px 15px;
		transform: translateY(100%);
		margin: 0;
		border: 0;
		background: none;
		outline: none;
		display: inline-block;
		border: none;
		border-radius: 4px;
		cursor: pointer;
		transition: transform .2s ease;
	}

	.spaneco-scrollup--visible {
		transform: translateY(0);
	}

	.spaneco-scrollup__content {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 10px;
		width: 18px;
		height: 18px;
		box-shadow: 0 1px 6px rgb(0 0 0 / 20%);
		color: #fff;
		background-color: #666666;
	}

	.spaneco-scrollup__icon {
		width: 15px;
		height: 10px;
	}
</style>