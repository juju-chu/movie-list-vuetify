<template>
    <v-app>
        <v-main>
            <SearchBar @fetch-movies="fetchMovies"/>
            <DataTable
                v-if="isFindMovies"
                :movieList="movieList"
            />
        </v-main>
    </v-app>
</template>

<script>
import axios from 'axios'
import { API_KEY, API_URL } from './common/constants'
import SearchBar from './components/SearchBar'
import DataTable from './components/DataTable'

export default {
    name: 'App',
    components: {
        SearchBar,
        DataTable,
    },
    data() {
        return {
            movieList: [],
            isFindMovies: false,
        }
    },
    methods: {
        fetchMovies(keyword) {
            axios.get(`${API_URL}/?apikey=${API_KEY}&s=${keyword.trim()}`).
                then(response => {
                    this.movieList = response.data.Search
                    this.isFindMovies = true
                }).catch(error => {
                    console.log(error)
                })
        }
    },
}
</script>
