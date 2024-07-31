<script lang="ts">
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
</script>

<main>
	<section class="carousel overflow-hidden">
		<div class="list h-full relative">
			{#each images as image}
				<article
					class="item absolute top-0 left-0 w-full h-full"
					style="background-color: {image.color};"
				>
					<div
						class="main-content h-full grid"
						style="grid-template-columns: calc(100% - calc(var(--w-image) * var(--calculate)))"
					>
						<div class="content p-[150px_20px_20px_80px]">
							<h2 class="text-[5em] font-aboreto">{image.name}</h2>
							<p class="price text-[3em] font-aboreto my-5">$ {image.price}</p>
							<p class="description">
								{image.description}
							</p>
							<button
								class="addToCard bg-[#4f8b69] text-white p-[10px_30px] font-poppins text-lg font-medium rounded-[30px] mt-5 border-none"
							>
								Add To Card
							</button>
						</div>
					</div>
					<figure
						class="image w-[var(--w-image)] h-full absolute top-0 left-[calc(100% - calc(var(--w-image) * var(--calculate)))] flex p-5 flex-col justify-end items-center font-medium"
					>
						<img
							class="w-[90%] mb-5 drop-shadow-[0_150px_50px_#9e0c0c55]"
							src="images/{image.src}"
							alt={image.alt}
							srcset=""
						/>
						<figcaption class="text-right mb-7 w-[70%] font-aboreto font-bold text-[1.3em]">
							{image.name}
						</figcaption>
					</figure>
				</article>
			{/each}
		</div>
		<div
			class="arrows absolute bottom-5 w-[calc(100% - calc(var(--w-image) * var(--calculate)))] grid grid-cols-2 grid-rows-1 justify-end gap-2 z-10"
		>
			<button
				class="bg-transparent border border-[var(--border-color)] text-white font-mono text-lg font-bold leading-none shadow-[0_10px_40px_#5555] cursor-pointer transition duration-500 hover:bg-[#eee5]"
				id="prev"
			></button>
			<button
				class="bg-transparent border border-[var(--border-color)] text-white font-mono text-lg font-bold leading-none shadow-[0_10px_40px_#5555] cursor-pointer transition duration-500 hover:bg-[#eee5]"
				id="next"
			></button>
		</div>
	</section>
</main>

<style>
	.carousel .list::before {
		width: var(--w-image);
		height: 100%;
		content: '';
		position: absolute;
		top: 0;
		left: calc(100% - calc(var(--w-image) * var(--calculate)));
		border-left: 1px solid var(--border-color);
		border-right: 1px solid var(--border-color);
		z-index: 10;
		pointer-events: none;
	}

	.carousel .list::after {
		position: absolute;
		top: 50px;
		left: 50px;
		content: '';
		background-color: red;
		width: 400px;
		height: 300px;
		z-index: 10;
		pointer-events: none;
		border-radius: 20px 50px 110px 230px;
		filter: blur(150px);
		opacity: 0.6;
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

		.carousel .list .item .image figcaption {
			color: #fff;
			width: 100%;
			text-align: center;
		}

		.carousel .list .item .main-content .content {
			display: none;
		}

		.arrows {
			left: 50%;
			justify-content: center;
		}
	}
</style>
