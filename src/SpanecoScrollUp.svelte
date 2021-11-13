<script lang="ts">
	import { onMount } from 'svelte';

	export let showOnPx: number = 150;
	export let clrbg: string = 'transparent';
	export let clrfill: string = '#ccc';
	export let clrfillhover: string = '#333';
	export let width: string = '50px';
	export let margin: string = '15px';
	export let position: number = 0;
	let progressPath;

	let visible: boolean = false;

	function scrollContainer() {
		return document.documentElement || document.body;
	}

	function getBodyHeight(): number {
		return Math.max(
				document.body.scrollHeight, document.documentElement.scrollHeight,
				document.body.offsetHeight, document.documentElement.offsetHeight,
				document.body.clientHeight, document.documentElement.clientHeight
		);
	}

	function getScrollTopHeight(): number {
		return window.pageYOffset || (document.documentElement || document.body.parentNode || document.body).scrollTop;
	}

	function getWindowHeight(): number {
		return window.innerHeight || (document.documentElement || document.body).clientHeight;
	}

	function getProgressPathLength(): number {
		return progressPath.getTotalLength();
	}

	function getProgressLength(): number {
		return Math.floor(getProgressPathLength() - (getScrollTopHeight() * getProgressPathLength() / (getBodyHeight() - getWindowHeight())));
	}

	function updateProgressPath(): void {
		progressPath.style.strokeDasharray = getProgressPathLength() + ' ' + getProgressPathLength();
		progressPath.style.strokeDashoffset = getProgressLength();
	}

	function handleOnScroll(): void {
		visible = false;

		if (!scrollContainer()) {
			return;
		}

		updateProgressPath();

		if (scrollContainer().scrollTop > showOnPx) {
			visible = true;
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

<button class="btn" class:is-visible={visible} on:click={scrollToTop} style="--clr-fill: {clrfill}; --clr-fill-hover: {clrfillhover};; --clr-bg: {clrbg}; --position: {position}; --width: {width}; --margin: {margin};">
	<svg class="bar" width="100%" height="100%" viewBox="-1 -1 102 102">
		<path class="bar__path" d="M50,1 a49,49 0 0,1 0,98 a49,49 0 0,1 0,-98" bind:this={progressPath}/>npm run
	</svg>
	<svg class="arrow" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24" width="24">
		<path d="M12 19V5M5 12l7-7 7 7"/>
	</svg>
</button>

<style>
	.btn {
		position: fixed;
		right: var(--position);
		bottom: 0;
		width: var(--width);
		height: var(--width);
		cursor: pointer;
		display: block;
		border-radius: 50px;
		box-shadow: inset  0 0 0 2px rgba(0,0,0,0.1);
		outline: none;
		border: none;
		z-index: 10000;
		opacity: 0;
		visibility: hidden;
		transform: translateY(15px);
		transition: all 200ms linear;
		background-color: var(--clr-bg);
		padding: 0;
		margin: var(--margin);
		color: var(--clr-fill);
	}

	.btn.is-visible {
		opacity: 1;
		visibility: visible;
		transform: translateY(0);
	}

	.btn:hover .arrow {
		transform: translate(-50%, -55%);
	}

	.arrow {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		color: inherit;
		stroke: currentColor;
		width: calc(var(--width)/2.3);
		fill: none;
		transition: all 200ms linear;
	}

	.bar {
		fill: none;
		stroke: currentColor;
		stroke-width: 4;
		transition: all 200ms linear;
	}

</style>