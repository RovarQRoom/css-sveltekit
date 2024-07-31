<script lang="ts">
	let hoveredIndex = -1;
	const images = new Array(5).fill(0).map((_, i) => `hover/img${i + 1}.jpg`);
</script>

<div class="flex w-full h-[100vh] justify-center items-center">
	<div class="list w-[1500px] grid grid-cols-5 grid-rows-[450px] gap-[50px]">
		{#each images as image, i}
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div
				class="item text-[#c7c3c3] font-poppins relative transition duration-500 cursor-pointer {hoveredIndex !==
					-1 && hoveredIndex !== i
					? 'blur-lg'
					: ''}"
				on:mouseenter={() => (hoveredIndex = i)}
				on:mouseleave={() => (hoveredIndex = -1)}
			>
				<img
					src="images/{image}"
					alt=""
					srcset=""
					class="w-full h-full rounded-[20px] object-cover transition duration-500 transform hover:scale-125"
				/>
				<div
					class="content absolute bottom-0 text-white p-[20px] transform translate-y-[100px] opacity-0 transition duration-500"
				>
					<h1 class="text-xl uppercase">Image {i + 1}</h1>
					<div class="description">
						Lorem, ipsum dolor sit amet consectetur adipisicing elit. Tenetur quidem et ducimus earum
						neque sapiente, eaque, mollitia laborum error commodi aliquid saepe dolor exercitationem
						optio aliquam. Aut laboriosam velit eveniet.
					</div>
				</div>
			</div>
		{/each}
	</div>
</div>

<style>
	.list {
		grid-template-columns: repeat(5, minmax(5, 1fr));
	}

	.list .item:hover .content {
		opacity: 1;
		transform: translateY(0);
	}
</style>
