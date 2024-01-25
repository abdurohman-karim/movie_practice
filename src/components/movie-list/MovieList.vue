<template>
    <ul class="list-group">
        <MovieItem v-for="movie in movies" v-bind:movie="movie" :key="movie.id" @onToggle="$emit('onToggle', $event)" @onDelete="$emit('onDelete', $event)" />
    </ul>
</template>
<script>
import MovieItem from "@/components/movie-item/MovieItem.vue"

export default {
    components: {
        MovieItem,
    },
    props: {
		movies: {
			type: Array,
			required: true,
		},
	},
}
</script>
<style>
    
</style>