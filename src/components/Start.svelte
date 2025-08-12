<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';
	import { createEventDispatcher } from 'svelte';
	import { welcomeTitle } from '../stores/wpTitles';
	const dispatch = createEventDispatcher();
	export let modalText;
	export let title;

	const enterCloudcities = () => {
		dispatch('enterCloudcities');
	};

	let textField;
	let button;
	onMount(async () => {
		//titleField = document.getElementById('title');
		textField = document.getElementById('text');
		button = document.getElementById('enter-button');
	});
	$: if (modalText.length > 0) {
		if (textField) {
			//textField.innerHTML = modalText[0];
			textField.innerHTML = '<p style="text-align: center;">A growing repository of cloud-based knowledges, activated by an interactive tool for pareidolic ecologies.</p><br><p style="text-align: center;">You are invited to draw upon the ocean of air and interpret a cloud’s multitudinal relations. A <i>mise-en-Aeroscène</i>: images of cloud formations are selected from the project’s growing repository, as collected by the artist Tomás Saraceno and your own willing contribution.</p>'
			button.innerHTML = modalText[1];
			button.children[0].style.margin = '0px';
		}
	}
</script>

<div class="modal-container" transition:fade={{ duration: 0 }}>
	<div class="modal-info">
		<h2 id="title" class="start-headline">Welcome to Cloud Cities</h2><!--{title}-->
		<div id="text" />
		<a class='buttonLink' href='https://cloudcities.studiotomassaraceno.org' target='_blank'><button id="readmore-button"><p>Read More</p></button></a>
		<button id="enter-button" on:click={enterCloudcities}>Enter</button>
	</div>
</div>

<style>
	.modal-container {
		z-index: 1000;
	}

	button {
		width: 100%;
	}

	@media (max-width: 500px) {
		.modal-info {
			width: 80%;
		}
	}
</style>
