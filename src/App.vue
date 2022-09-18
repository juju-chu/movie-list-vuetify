<template>
    <v-app>
        <v-main>
            <SearchBar @fetch-movies="fetchMovies"/>
        </v-main>
    </v-app>
</template>

<script>
import axios from 'axios'
import { API_I, API_KEY, API_URL } from './common/constants'
import SearchBar from './components/SearchBar'

export default {
    name: 'App',
    
    components: {
        SearchBar,
    },
    data() {
        return {
            movieList: [],
        }
    },
    methods: {
        fetchMovies(keyword) {
            axios.get(`${API_URL}/?i=${API_I}&apikey=${API_KEY}&s=${keyword.trim()}`).
                then(response => {
                    this.movieList = response.data.Search
                }).catch(error => {
                    console.log(error)
                })
        }
    },
}
</script>
