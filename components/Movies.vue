<template>
	<div id='movies'>
		<div class="max-w-[1400px] my-0 mx-auto flex py-8 px-4">
			<input
				v-model.lazy="searchInput"
				type="text"
				placeholder="Search"
				class="
					max-w-[350px]
					w-full
					py-3
					px-2
					text-xs
					border-0
					focus:outline-none
				"
				:class="searchInput === '' ? 'rounded' : 'rounded-l'"
				@keyup.enter="$fetch"
			/>
			<button
				v-show="searchInput !== ''"
				class="
					rounded-r
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
				"
				@click="clearSearch"
			>
				Clear Search
			</button>
		</div>

		<!-- Loading Animation -->
		<div v-if="$fetchState.pending" class="flex items-center justify-center h-32">
			<Loading />
		</div>

		<!-- Movies -->
		<div v-else class="max-w-[1400px] my-0 mx-auto py-8 px-4">
			<!-- Search Results -->
			<div v-if="searchInput !== ''" id="movie-grid" class="flex flex-wrap">
				<div
					v-for="(movie, index) in searchedMovies"
					:key="index"
					class="flex flex-col p-4 w-full sm:w-1/2 md:w-1/3 lg:w-1/4"
				>
					<div class="relative overflow-hidden group">
						<img
							:src="
								movie.poster_path
									? `https://image.tmdb.org/t/p/w500/${movie.poster_path}`
									: 'https://via.placeholder.com/254x381'
							"
							alt=""
							class="block w-full h-full"
						/>
						<p
							class="
								absolute
								top-0
								left-0
								flex
								justify-center
								items-center
								w-10
								h-10
								bg-[#c92502]
								text-white
								rounded-br-2xl
							"
						>
							{{ movie.vote_average }}
						</p>
						<p
							class="
								leading-normal
								absolute
								bottom-0
								bg-[#c92502] bg-opacity-90
								p-3
								text-white
								transform
								translate-y-full
								ease-in-out
								duration-300
								transition-all
								group-hover:translate-y-0
							"
						>
							{{ movie.overview }}
						</p>
					</div>
					<div class="mt-auto">
						<p class="mt-2 text-white text-xl">
							{{ movie.title.slice(0, 25)
							}}<span v-if="movie.title.length > 25">...</span>
						</p>
						<p class="mt-2 text-[#c9c9c9]">
							Released:
							{{
								new Date(movie.release_date).toLocaleString('en-us', {
									month: 'long',
									day: 'numeric',
									year: 'numeric',
								})
							}}
						</p>
						<NuxtLink
							class="
								inline-block
								no-underline
								bg-[#c92502]
								border-0
								py-2
								px-4
								text-white
								rounded
								cursor-pointer
								transition-all
								ease-linear
								duration-[300ms]
								mt-2
							"
							:to="{ name: 'movies-id', params: { id: movie.id } }"
						>
							Get More Info
						</NuxtLink>
					</div>
				</div>
			</div>

			<!-- Now Streaming  -->
			<div v-else id="movie-grid" class="flex flex-wrap">
				<div
					v-for="(movie, index) in movies"
					:key="index"
					class="flex flex-col p-4 w-full sm:w-1/2 md:w-1/3 lg:w-1/4"
				>
					<div class="relative overflow-hidden group">
						<img
							:src="
								movie.poster_path
									? `https://image.tmdb.org/t/p/w500/${movie.poster_path}`
									: 'https://via.placeholder.com/254x381'
							"
							alt=""
							class="block w-full h-full"
						/>
						<p
							class="
								absolute
								top-0
								left-0
								flex
								justify-center
								items-center
								w-10
								h-10
								bg-[#c92502]
								text-white
								rounded-br-2xl
							"
						>
							{{ movie.vote_average }}
						</p>
						<p
							class="
								leading-normal
								absolute
								bottom-0
								bg-[#c92502] bg-opacity-90
								p-3
								text-white
								transform
								translate-y-full
								ease-in-out
								duration-300
								transition-all
								group-hover:translate-y-0
							"
						>
							{{ movie.overview }}
						</p>
					</div>
					<div class="mt-auto">
						<p class="mt-2 text-white text-xl">
							{{ movie.title.slice(0, 25)
							}}<span v-if="movie.title.length > 25">...</span>
						</p>
						<p class="mt-2 text-[#c9c9c9]">
							Released:
							{{
								new Date(movie.release_date).toLocaleString('en-us', {
									month: 'long',
									day: 'numeric',
									year: 'numeric',
								})
							}}
						</p>
						<NuxtLink
							class="
								inline-block
								no-underline
								bg-[#c92502]
								border-0
								py-2
								px-4
								text-white
								rounded
								cursor-pointer
								transition-all
								ease-linear
								duration-[300ms]
								mt-2
							"
							:to="{ name: 'movies-id', params: { id: movie.id } }"
						>
							Get More Info
						</NuxtLink>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	data() {
		return {
			movies: [],
			searchedMovies: [],
			searchInput: '',
		}
	},
	async fetch() {
		if (this.searchInput === '') {
			await this.getMovies()
			return
		}
		await this.searchMovies()
	},
	watch: {
		searchInput() {
			// eslint-disable-next-line no-console
			console.log(this.searchInput)
		},
	},
	methods: {
		async getMovies() {
			const data = axios.get(
				`https://api.themoviedb.org/3/movie/now_playing?api_key=36cbf2184c1559b30cfeacbe0f5846af&language=en-US&page=1`
			)
			const result = await data
			result.data.results.forEach((movie) => {
				this.movies.push(movie)
			})
		},
		async searchMovies() {
			const data = axios.get(
				`https://api.themoviedb.org/3/search/movie?api_key=36cbf2184c1559b30cfeacbe0f5846af&language=en-US&page=1&query=${this.searchInput}`
			)
			const result = await data
			result.data.results.forEach((movie) => {
				this.searchedMovies.push(movie)
			})
		},
		clearSearch() {
			this.searchInput = ''
			this.searchedMovies = []
		},
	},
}
</script>

<style scoped></style>
