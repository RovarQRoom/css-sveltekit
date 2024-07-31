<script lang="ts">
	import { onMount } from 'svelte';
	let carousel: HTMLElement;
	let items: NodeListOf<Element>;
	let itemsCount: number;
	let active = 1;
	let other_1: number | null = null;
	let other_2: number | null = null;

	let currentIndex = 1;
	let colors = ['#9c4d2f', '#f5bfaf', '#dedfe1', '#7eb63d'];
	const images = new Array(4).fill(0).map((_, i) => {
		return {
			src: `slider/${i + 1}.png`,
			alt: `Coffee ${i + 1}`,
			name: `Coffee ${i + 1}`,
			price: (20 * (i + 1)).toFixed(2),
			description: `Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolor, tempore quos, placeat totam ut animi magni voluptas modi quidem facere dolorum aliquid officiis, doloremque aliquam. Sit maxime voluptatum quam ex.`,
			color: colors[i]
		};
	});

	onMount(() => {
		items = carousel.querySelectorAll('.item');
		itemsCount = items.length;

		// autoPlay = setInterval(() => {
		// 	next();
		// }, 5000);
	});

	function getClass(index: number) {
		if (index === currentIndex) return 'active';
	}

	function prev() {
		if (!carousel) return;
		currentIndex = (currentIndex - 1 + images.length) % images.length;
		carousel.classList.remove('next');
		carousel.classList.add('prev');
		active = active - 1 < 0 ? itemsCount - 1 : active - 1;
		other_1 = active + 1 >= itemsCount ? 0 : active + 1;
		other_2 = other_1 + 1 >= itemsCount ? 0 : other_1 + 1;
		changeSlider();
	}

	function next() {
		if (!carousel) return;
		currentIndex = (currentIndex + 1) % images.length;
		carousel.classList.remove('prev');
		carousel.classList.add('next');
		active = active + 1 >= itemsCount ? 0 : active + 1;
		other_1 = active - 1 < 0 ? itemsCount - 1 : active - 1;
		other_2 = active + 1 >= itemsCount ? 0 : active + 1;
		changeSlider();
	}

	const changeSlider = () => {
		if (!carousel) return;
		let itemOldActive = document.querySelector('.carousel .item.active');
		if (itemOldActive) itemOldActive.classList.remove('active');

		let itemOldOther_1 = document.querySelector('.carousel .item.other_1');
		if (itemOldOther_1) itemOldOther_1.classList.remove('other_1');

		let itemOldOther_2 = document.querySelector('.carousel .item.other_2');
		if (itemOldOther_2) itemOldOther_2.classList.remove('other_2');

		items.forEach((e) => {
			const image = e.querySelector('.image img') as HTMLImageElement;
			if (image) {
				image.style.animation = 'none';
			}
			const figCaption = e.querySelector('.image figcaption') as HTMLElement;
			if (figCaption) {
				figCaption.style.animation = 'none';
			}
			void (e as HTMLElement).offsetWidth;
			const image2 = e.querySelector('.image img') as HTMLImageElement;
			if (image2) {
				image2.style.animation = '';
			}
			const figCaption2 = e.querySelector('.image figcaption') as HTMLElement;
			if (figCaption2) {
				figCaption2.style.animation = '';
			}
		});

		items[active].classList.add('active');
		if (other_1 !== null) {
			items[other_1].classList.add('other_0');
		}
		if (other_2 !== null) {
			items[other_2].classList.add('other_0');
		}

		// clearInterval(autoPlay);
		// autoPlay = setInterval(() => {
		// 	next();
		// }, 5000);
	};
	let autoPlay: number;
</script>

<main>
	<section bind:this={carousel} class="carousel next w-full h-[100vh]">
		<div class="list w-full h-full">
			{#each images as image, index}
				<article class="item {getClass(index)} h-full w-full">
					<div class="main-content w-full h-full justify-center items-center" style="background-color: {image.color};">
						<!-- <div class="content">
							<h2>{image.name}</h2>
							<p class="price">$ {image.price}</p>
							<p class="description">
								{image.description}
							</p>
							<button class="addToCard"> Add To Card </button>
						</div> -->
                        <!-- <figure class="image  w-full flex justify-center"> -->
                        <div class="w-full flex h-full justify-center items-center">
                            <img src="images/{image.src}" class="w-96 object-cover " alt="" />
                            <!-- <figcaption>{image.name}</figcaption> -->
                        </div>
                        <!-- </figure> -->
					</div>
				</article>
			{/each}
		</div>
		<div class="arrows">
			<button id="prev" on:click={prev}>{'<'}</button>
			<button id="next" on:click={next}>{'>'}</button>
		</div>
	</section>
</main>

<style>
	.arrows {
		position: absolute;
		bottom: 20px;
		width: calc(100% - calc(var(--w-image) * var(--calculate)));
		display: grid;
		grid-template-columns: repeat(2, 50px);
		grid-template-rows: 50px;
		justify-content: end;
		gap: 10px;
	}
	.arrows button {
		background-color: transparent;
		border: 1px solid var(--border-color);
		color: #fff;
		font-family: monospace;
		font-size: large;
		font-weight: bold;
		line-height: 0;
		box-shadow: 0 10px 40px #5555;
		cursor: pointer;
		transition: 0.5s;
	}
	.arrows button:hover {
		background-color: #eee5;
	}
	.carousel .list .item {
		display: none;
	}
	.carousel .list .item.active,
	.carousel .list .item.other_1,
	.carousel .list .item.other_2 {
		display: block;
	}
	.carousel .list .item.active {
		z-index: 2;
	}
	.carousel .list .item.other_1,
	.carousel .list .item.other_2 {
		pointer-events: none;
	}
	.carousel .list .item.active .main-content {
		animation: showContent 1s ease-in-out 1 forwards;
	}
	@keyframes showContent {
		from {
			clip-path: circle(0% at 70% 50%);
		}
		to {
			clip-path: circle(100% at 70% 50%);
		}
	}
	.next .item.other_1 {
		z-index: 1;
	}
	.next .item .image img,
	.next .item .image figcaption {
		animation: effectNext 0.5s ease-in-out 1 forwards;
	}
	@keyframes effectNext {
		from {
			transform: translateX(calc(var(--transform-from)));
		}
		to {
			transform: translateX(calc(var(--transform-from) - var(--w-image)));
		}
	}
	.next .item.active .image {
		--transform-from: var(--w-image);
	}
	.next .item.other_1 .image {
		z-index: 3;
		--transform-from: 0px;
		overflow: hidden;
	}
	.next .item.other_2 .image {
		z-index: 3;
		--transform-from: calc(var(--w-image) * 2);
	}
	.arrows {
		z-index: 10;
	}
	/* prev */
	.prev .list .item .image img,
	.prev .list .item .image figcaption {
		animation: effectPrev 0.5s ease-in-out 1 forwards;
	}
	@keyframes effectPrev {
		from {
			transform: translateX(calc(var(--transform-from)));
		}
		to {
			transform: translateX(calc(var(--transform-from) + var(--w-image)));
		}
	}
	.prev .list .item.active .image {
		--transform-from: calc(var(--w-image) * -1);
		overflow: hidden;
	}
	.prev .list .item.other_1 .image {
		--transform-from: 0px;
		z-index: 3;
	}
	.prev .list .item.other_2 .image {
		z-index: 3;
		--transform-from: var(--w-image);
	}
	.prev .list .item.other_2 .main-content {
		opacity: 0;
	}
	@media screen and (max-width: 1023px) {
		:root {
			--calculate: 1;
			--w-image: 400px;
		}
		.carousel .list .item .main-content .content h2 {
			font-size: 3em;
		}
	}
	@media screen and (max-width: 767px) {
		.carousel .list .item .image {
			width: 100%;
			left: 0;
			justify-content: center;
		}
	}
</style>
