<template>
	<div id="hero">
		<Hero />
		<Movies />
	</div>
</template>

<script>
export default {
	head() {
		return {
			title: 'Movie App - Latest Streaming Movie Info',
			meta: [
				{
					hid: 'description',
					name: 'description',
					content: 'Get all the latest streaming movies in theaters & online',
				},
				{
					hid: 'keywords',
					name: 'keywords',
					content: 'movies, stream, stremaing',
				},
			],
		}
	},
}
</script>
