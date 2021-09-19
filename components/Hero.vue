<template>
	<div class="h-[500px] relative">
		<img
			src="../assets/img/movieHero.jpg"
			alt="Hero"
			class="w-full h-full object-cover"
		/>
		<div class="absolute top-0 left-0 h-full w-full bg-black bg-opacity-60" />
		<div
			class="z-50 absolute top-0 m-0 h-full w-full flex flex-col justify-center"
		>
			<div class="px-4 w-full mx-auto max-w-[1400px]">
				<span
					class="
						font-semibold
						text-lg
						md:text-2xl
						uppercase
						text-[#c92502]
						mb-2
					"
					>Now Stremaing</span
				>
				<h1 class="text-white text-6xl md:text-8xl font-extralight mb-2">
					<span class="font-medium">Now</span> Streaming
				</h1>
				<a href="#movies" class="text-xl self-start inline-block no-underline bg-[#c92502] border-0 py-2 px-4 text-white rounded cursor-pointer transition-all ease-linear duration-[300ms]">View Movies</a>
			</div>
		</div>
	</div>
</template>

<script>
export default {}
</script>

<style scoped></style>
