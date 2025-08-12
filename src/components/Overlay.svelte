<script>
	import { createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();

	export let openComponent;
	export let image;

	const closeLargeImage = () => {
		closeButtonSrc = 'button_close.svg';
		dispatch('closeLargeImage');
	};
	const closeGalleryRemix = () => {
		closeButtonSrc = 'button_close.svg';
		openComponent = 'gallery';
		console.log(openComponent);
		dispatch('closeGalleryRemix');
	};

	const clickDraw = () => {
		dispatch('clickDraw');
	};

	let drawButtonSrc = 'button_draw.svg';
	let aboutButtonSrc = 'button_about.svg';
	let closeButtonSrc = 'button_close.svg';
</script>

<div class="overlay">
	{#if openComponent === 'gallery'}
		<a href="/draw">
			<img
				src={drawButtonSrc}
				alt="draw-button"
				class="icon-button draw-button"
				on:click={clickDraw}
				on:mouseenter={() => (drawButtonSrc = 'button_draw_hover.svg')}
				on:mouseleave={() => (drawButtonSrc = 'button_draw.svg')}
			/>
		</a>
		<!--h2>Cloud Cities</h2-->
		<a href="https://cloudcities.studiotomassaraceno.org" target="_blank">
			<img
				src={aboutButtonSrc}
				alt="about-button"
				class="icon-button about-button"
				on:mouseenter={() => (aboutButtonSrc = 'button_about_hover.svg')}
				on:mouseleave={() => (aboutButtonSrc = 'button_about.svg')}
			/>
		</a>
	{/if}
	{#if openComponent === 'individualPost'}
		<a>
			<img
				class="icon-button"
				alt="exit-button"
				src={closeButtonSrc}
				on:click={closeLargeImage}
				on:mouseenter={() => (closeButtonSrc = 'button_close_hover.svg')}
				on:mouseleave={() => (closeButtonSrc = 'button_close.svg')}
			/>
		</a>
		<h2>{image.title}</h2>
		<a href="https://cloudcities.studiotomassaraceno.org">
			<img
				src={aboutButtonSrc}
				alt="about-button"
				class="icon-button"
				on:mouseenter={() => (aboutButtonSrc = 'button_about_hover.svg')}
				on:mouseleave={() => (aboutButtonSrc = 'button_about.svg')}
			/>
		</a>
	{/if}
	{#if openComponent === 'remix'}
		<a>
			<img
				class="icon-button"
				alt="exit-button"
				src={closeButtonSrc}
				on:click={closeGalleryRemix}
				on:mouseenter={() => (closeButtonSrc = 'button_close_hover.svg')}
				on:mouseleave={() => (closeButtonSrc = 'button_close.svg')}
			/>
		</a>
		<h2 id="remix"><!--Select a drawing to re/interpret--></h2>
	{/if}
</div>

<style>
	.overlay {
		color: white;
		width: 100%;
		position: fixed;
		top: 0px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		z-index: 1000;
	}

	.icon-button {
		position: initial;
	}

	h2 {
		font-style: normal;
		font-weight: 400;
		font-size: 32px;
		line-height: 38px;
		/*text-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);*/
		margin: 0px;
		max-width: 60vw;
		word-wrap: break-word;
		position: absolute;
		left: 50%;
		transform: translateX(-50%);
		top: 16px;
	}

	#remix {
		position: absolute;
		top: 10px;
		left: 50%;
		transform: translateX(-50%);
	}

	@media (max-width: 500px) {
		h2 {
			font-style: normal;
			font-weight: 400;
			font-size: 18px;
			line-height: 20px;
			text-align: center;
			letter-spacing: 0.05em;
			top: 10px;
		}
	}
	@media (max-height: 500px) {
		h2 {
			font-style: normal;
			font-weight: 400;
			font-size: 18px;
			line-height: 20px;
			text-align: center;
			letter-spacing: 0.05em;
			top: 10px;
		}
	}
</style>
