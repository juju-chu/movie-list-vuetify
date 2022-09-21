<template>
    <v-app>
        <v-main>
            <SearchBar @fetch-movies="fetchMovies"/>
            <DataTable
                v-if="isFindMovies"
                :movieList="movieList"
            />
            <PagiNation
                v-if="isFindMovies"
                :pageLength="pageLength"
                @update-page="updatePage"
            />
        </v-main>
    </v-app>
</template>

<script>
import axios from 'axios'
import { API_KEY, API_URL } from './common/constants'
import SearchBar from './components/SearchBar'
import DataTable from './components/DataTable'
import PagiNation from './components/PagiNation'

export default {
    name: 'App',
    components: {
        SearchBar,
        DataTable,
        PagiNation,
    },
    data() {
        return {
            allMovies: [],
            movieList: [],
            isFindMovies: false,
            perPage: 10,
            pageLength: 0,
        }
    },
    methods: {
        fetchMovies(keyword) {
            axios.get(`${API_URL}/?apikey=${API_KEY}&s=${keyword.trim()}`).
                then(response => {
                    const movies = response.data.Search
                    this.allMovies = movies.map((movie) => ({
                        title: movie.Title,
                        type: movie.Type,
                        year: movie.Year,
                        id: movie.imdbID,
                    }))

                    this.isFindMovies = true
                    this.pageLength = Math.ceil(this.allMovies.length / this.perPage)
                    this.updatePage(1)
                }).catch(error => {
                    console.log(error)
                })
        },
        updatePage(pageIndex) {
            this.movieList = this.allMovies.slice((pageIndex - 1) * this.perPage, pageIndex * this.perPage)
        },
    },
}
</script>
