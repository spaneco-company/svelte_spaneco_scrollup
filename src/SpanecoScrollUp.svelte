<script lang="ts">
	import { onMount } from 'svelte';

	export let showOnPx: number = 150;
	export let clricon: string = '#fff';
	export let clrbg: string = '#45A1B0';
	export let width: number = 15;
	export let radius: string = '0';

	let hidden: boolean = true;

	$:position = $$props.left === undefined ?  0 : 1;
	$:radius = $$props.round === undefined ?  radius : '50%';

	function scrollContainer() {
		return document.documentElement || document.body;
	}

	function handleOnScroll(): void {
		if (!scrollContainer()) {
			return;
		}

		if (scrollContainer().scrollTop > showOnPx) {
			hidden = false;
		} else {
			hidden = true;
		}
	}

	function scrollToTop(): void {
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
<svelte:options tag="spaneco-scrollup" />

<svelte:window on:scroll="{handleOnScroll}" />

<button class="btn" class:is-visible={!hidden} aria-hidden="true" tabindex="-1" on:click={scrollToTop} style="--position: {position};">
	<div class="content" style="--clr-icon: {clricon}; --clr-bg: {clrbg}; --width: {`${width}px`}; --radius: {`${radius}`};">
		<svg class="icon" viewBox="0 0 17 10">
			<path d="M14.692 9.63a1.211 1.211 0 101.712-1.715L9.602 1.121a1.211 1.211 0 00-1.714.002L1.113 7.917a1.211 1.211 0 101.716 1.71l5.919-5.935 5.944 5.937z"></path>
		</svg>
	</div>
</button>

{JSON.stringify($$props)}


<style>
	.btn {
		position: fixed;
		right: var(--position);
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
		cursor: pointer;
		transition: transform .2s ease;
	}

	.btn.is-visible {
		transform: translateY(0);
	}

	.content {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 10px;
		width: var(--width);
		height: var(--width);
		box-shadow: 0 1px 6px rgb(0 0 0 / 20%);
		color: var(--clr-icon);
		background-color: var(--clr-bg);
		border-radius: var(--radius);
	}

	.icon {
		width: 15px;
		height: 10px;
		fill: currentColor;
	}
</style>