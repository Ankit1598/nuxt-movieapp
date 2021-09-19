<template>
	<div
		v-if="$fetchState.pending"
		class="flex items-center justify-center h-screen"
	>
		<Loading />
	</div>

	<div
		v-else
		class="
			max-w-[1400px]
			my-0
			mx-auto
			text-white
			min-h-screen
			flex flex-col
			justify-center
			py-8
			px-4
		"
	>
		<NuxtLink
			class="
				rounded
				inline-block
				no-underline
				bg-[#c92502]
				border-0
				py-2
				px-4
				text-white
				cursor-pointer
				transition-all
				ease-linear
				duration-[300ms]
				self-start
				mb-8
			"
			:to="{ name: 'index' }"
		>
			Back
		</NuxtLink>
		<div
			class="
				flex
				items-center
				flex-col
				md:flex-row md:items-start
				text-white
				gap-8
			"
		>
			<div class="w-1/2 md:w-1/3">
				<img
					:src="`https://image.tmdb.org/t/p/w500/${movie.poster_path}`"
					alt=""
					class="max-h-[500px] md:max-h-[700px] w-full"
				/>
			</div>
			<div class="w-full md:w-2/3">
				<h1 class="text-6xl font-normal">Title: {{ movie.title }}</h1>
				<p class="mt-3 text-xl italic">
					<span class="font-semibold not-italic">Tagline:</span> "{{
						movie.tagline
					}}"
				</p>
				<p class="mt-3 text-xl">
					<span class="font-semibold underline">Released:</span>
					{{
						new Date(movie.release_date).toLocaleString('en-us', {
							month: 'long',
							day: 'numeric',
							year: 'numeric',
						})
					}}
				</p>
				<p class="mt-3 text-xl">
					<span class="font-semibold underline">Duration:</span>
					{{ movie.runtime }} minutes
				</p>
				<p class="mt-3 text-xl">
					<span class="font-semibold underline">Revenue:</span>
					{{
						movie.revenue.toLocaleString('en-us', {
							style: 'currency',
							currency: 'USD',
						})
					}}
				</p>
				<p class="mt-3 text-xl">
					<span class="font-semibold underline">Overview:</span>
					{{ movie.overview }}
				</p>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	data() {
		return {
			movie: '',
		}
	},
	async fetch() {
		await this.getSingleMovie()
	},
	head() {
		return {
			title: this.movie.title,
		}
	},
	methods: {
		async getSingleMovie() {
			const data = axios.get(
				`https://api.themoviedb.org/3/movie/${this.$route.params.id}?api_key=36cbf2184c1559b30cfeacbe0f5846af&language=en-US`
			)
			const result = await data
			this.movie = result.data
		},
	},
}
</script>

<style scoped></style>
