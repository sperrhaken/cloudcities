<script>
	import { onMount } from 'svelte';
	import { createEventDispatcher } from 'svelte';
	import { getImages } from '../stores/repoImages';
	const dispatch = createEventDispatcher();
	export let repoImages;
	export let remix;

	const openImage = (e) => {
		if (remix) {
			dispatch('drawOnImage', e);
		} else {
			dispatch('openImage', e);
		}
	};

	let imageDetail = true;

	const showImageDetail = (e) => {
		if (createInfoModals) {
			if (imageDetail) {
				e.srcElement.children[1].style.visibility = 'visible';
				e.srcElement.children[0].children[0].style.transform = 'scale(1.1)';
				e.srcElement.children[0].children[0].style.filter = 'blur(6px)';
			} else {
				e.srcElement.children[1].style.visibility = 'hidden';
				e.srcElement.children[0].children[0].style.transform = 'scale(1)';
				e.srcElement.children[0].children[0].style.filter = 'blur(0px)';
			}
			imageDetail = !imageDetail;
		}
	};

	//load on scroll
	let count = 20;
	let createInfoModals = true;

	let columns;

	const resizeGallery = () => {
		if (window.innerWidth <= 500) {
			columns = 1;
		} else if (window.innerWidth > 500 && window.innerWidth <= 700) {
			columns = 2;
		} else if (window.innerWidth > 700 && window.innerWidth <= 1500) {
			columns = 3;
		} else {
			columns = 4;
		}
	};

	onMount(async () => {
		resizeGallery();
		window.onresize = resizeGallery;
		//don't create info modals on mobile

		if (window.screen.width <= 560) {
			//createInfoModals = false;
			window.addEventListener('scroll', scrollFunction);
		}
		let gallery = document.getElementById('gallery');
		let scroll = true;
		let lastScrollTop = 0;

		gallery.addEventListener('scroll', scrollFunction);
		function scrollFunction() {
			let st = window.pageXOffset || gallery.scrollTop;
			if (st < lastScrollTop) {
				//detect scroll up
				return;
			} else if (gallery.scrollTop + window.innerHeight > gallery.scrollHeight - 100 && scroll) {
				if (count > repoImages) {
					//we request more images than are in the repo so stop
					return;
				}
				count += 10;
				getImages(count);

				scroll = false;
			} else {
				scroll = true;
			}
			lastScrollTop = st <= 0 ? 0 : st;
		}

		let imgContainer = document.getElementsByClassName('gallery-item');
		for (let i = 0; i < imgContainer.length; i++) {

			/*if(i == 0){
				imgContainer[i].classList.add('bigGalleryImage');
				console.log(imgContainer[i],i);
			}*/

			imgContainer[i].addEventListener('mouseover', function (e) {});
		}
	});
</script>

<div class="gallery" id="gallery">
	{#each repoImages as image, i}
		<!--1 column: none, 2: 0, 3: 0, 4: 0-->
		<!--2: {#if i == 0 | i == 5 | i == 10 | i == 15 | i == 20 | i == 25 } -->
		<!--3: {#if i == 0 | i == 4 | i == 6 | i == 10 | i == 12 | i == 16 } -->
		<!--4: {#if i == 0 | i == 7 | i == 10 | i == 17 | i == 20 | i == 27 } -->
		{#if (columns == 4 && i == 0 | i % 10 == 0 | (i-7) % 10 == 0)
			|| (columns == 3 && i == 0 | (i+2) % 6 == 0 | i % 6 == 0)
			|| (columns == 2 && i == 0 | i % 5 == 0) }
		<div class="gallery-container bigImageGallery" on:click={openImage(image)}>
			<!--p>{i}</p-->
			<div class="gallery-item" on:mouseenter={showImageDetail} on:mouseleave={showImageDetail}>
				<div class="image">
					<img src={image.link} alt="" />
				</div>
				{#if createInfoModals}
					<div class="img-info" style="visibility: hidden">
						<div class="info-text">
							<h2 class="desc-text">{image.title}</h2>
							<p>{image.date}</p>
							{#if image.location !== 'undefined'}
								<p>{image.location}</p>
							{/if}
						</div>
					</div>
				{/if}
			</div>
		</div>
		{:else}
		<div class="gallery-container" on:click={openImage(image)}>
			<div class="gallery-item" on:mouseenter={showImageDetail} on:mouseleave={showImageDetail}>
				<div class="image">
					<img src={image.link} alt="" />
				</div>
				{#if createInfoModals}
					<div class="img-info" style="visibility: hidden">
						<div class="info-text">
							<h2 class="desc-text">{image.title}</h2>
							<p>{image.date}</p>
							{#if image.location !== 'undefined'}
								<p>{image.location}</p>
							{/if}
						</div>
					</div>
				{/if}
			</div>
		</div>
		{/if}
	{/each}
	<!--div class="flex-container" id="flex-container1">
		{#each repoImages as image, i}
			{#if i % columns == 0}
				<div class="gallery-container" on:click={openImage(image)}>
					<div class="gallery-item" on:mouseenter={showImageDetail} on:mouseleave={showImageDetail}>
						<div class="image">
							<img src={image.link} alt="" />
						</div>
						{#if createInfoModals}
							<div class="img-info" style="visibility: hidden">
								<div class="info-text">
									<p>{image.date}</p>
									{#if image.location !== 'undefined'}
										<p>{image.location}</p>
									{/if}
									<h2 class="desc-text">{image.title}</h2>
								</div>
							</div>
						{/if}
					</div>
				</div>
			{/if}
		{/each}
	</div>
	{#if columns == 2 || columns == 3 || columns == 4}
		<div class="flex-container" id="flex-container2">
			{#each repoImages as image, i}
				{#if i % columns == 1}
					<div class="gallery-container" on:click={openImage(image)}>
						<div
							class="gallery-item"
							on:mouseenter={showImageDetail}
							on:mouseleave={showImageDetail}
						>
							<div class="image">
								<img src={image.link} alt="" />
							</div>
							{#if createInfoModals}
								<div class="img-info" style="visibility: hidden">
									<div class="info-text">
										<p>{image.date}</p>
										{#if image.location !== 'undefined'}
											<p>{image.location}</p>
										{/if}
										<h2 class="desc-text">{image.title}</h2>
									</div>
								</div>
							{/if}
						</div>
					</div>
				{/if}
			{/each}
		</div>
	{/if}
	{#if columns == 3 || columns == 4}
		<div class="flex-container" id="flex-container3">
			{#each repoImages as image, i}
				{#if i % columns == 2}
					<div class="gallery-container" on:click={openImage(image)}>
						<div
							class="gallery-item"
							on:mouseenter={showImageDetail}
							on:mouseleave={showImageDetail}
						>
							<div class="image">
								<img src={image.link} alt="" />
							</div>
							{#if createInfoModals}
								<div class="img-info" style="visibility: hidden">
									<div class="info-text">
										<p>{image.date}</p>
										{#if image.location !== 'undefined'}
											<p>{image.location}</p>
										{/if}
										<h2 class="desc-text">{image.title}</h2>
									</div>
								</div>
							{/if}
						</div>
					</div>
				{/if}
			{/each}
		</div>
	{/if}
	{#if columns == 4}
		<div class="flex-container" id="flex-container4">
			{#each repoImages as image, i}
				{#if i % columns == 3}
					<div class="gallery-container" on:click={openImage(image)}>
						<div
							class="gallery-item"
							on:mouseenter={showImageDetail}
							on:mouseleave={showImageDetail}
						>
							<div class="image">
								<img src={image.link} alt="" />
							</div>
							{#if createInfoModals}
								<div class="img-info" style="visibility: hidden">
									<div class="info-text">
										<p>{image.date}</p>
										{#if image.location !== 'undefined'}
											<p>{image.location}</p>
										{/if}
										<h2 class="desc-text">{image.title}</h2>
									</div>
								</div>
							{/if}
						</div>
					</div>
				{/if}
			{/each}
		</div>
	{/if}-->
</div>

<style>
	/*BRING OVER TO GLOBAL.CSS*/

	.img-info {
		position: absolute;
		left: 0px;
		top: 0px;
		width: 100%;
		height: 100%;
		align-items: flex-start;
		text-align: start;
		background-color: rgb(0, 0, 0, 0.24);
		color: white;
	}

	.info-text {
		position: absolute;
		bottom: 10px;
		margin-left: 10px;
	}

	.info-text > h2 {
		margin-top: 5px;
		margin-bottom: 10px;
		display: flex;
		text-align: left;
		line-height: unset;
	}
	.info-text > p {
		font-size: 20px;
	}

	p {
		margin: 0px;
		font-size: unset;
		line-height: unset;
	}

	.desc-text {
		margin-top: 5px;
		font-size: 20px;
	}

	.gallery-container:hover {
		border: 1px solid white;
	}
	img:hover {
		filter: blur(6px);
		transform: scale(1.1);
		-ms-transform: scale(1.1);
		-moz-transform: scale(1.1);
		-webkit-transform: scale(1.1);
		-o-transform: scale(1.1);
	}

	@media (max-width: 1500px) {
		.gallery {
			grid-template-columns: 1fr 1fr 1fr;
		}
	}

	@media (max-width: 700px) {
		.gallery {
			grid-template-columns: 1fr 1fr;
		}
		.gallery-item:hover > .img-info {
			visibility: visible!important;
		}
		.gallery-item:hover img {
			filter: blur(6px)!important;
			transform: scale(1.1)!important;
			-ms-transform: scale(1.1)!important;
			-moz-transform: scale(1.1)!important;
			-webkit-transform: scale(1.1)!important;
			-o-transform: scale(1.1)!important;
		}
	}

	@media (max-width: 500px) {
		.gallery {
			grid-template-columns: 1fr;
		}
	}
</style>
