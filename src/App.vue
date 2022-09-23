<template>
    <v-app>
        <v-main>
            <SearchBar @fetch-movies="fetchMovies"/>
            <DataTable
                v-if="isFindMovies"
                :movieList="movieList"
            />
            <LoadIng v-if="isLoading"/>
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
import LoadIng from './components/LoadIng'
import PagiNation from './components/PagiNation'

export default {
    name: 'App',
    components: {
        SearchBar,
        DataTable,
        LoadIng,
        PagiNation,
    },
    data() {
        return {
            allMovies: [],
            movieList: [],
            type: '',
            isFindMovies: false,
            isLoading: false,
            perPage: 10,
            pageLength: 0,
        }
    },
    methods: {
        fetchMovies(value) {
            const keyword = value[0].trim()
            this.type = value[1] || ''
            this.isLoading = true
            
            axios.get(`${API_URL}/?apikey=${API_KEY}&s=${keyword}`).
                then(response => {
                    const movies = response.data.Search
                    this.allMovies = movies.map((movie) => ({
                        title: movie.Title,
                        type: movie.Type,
                        year: movie.Year,
                        id: movie.imdbID,
                    }))

                    this.allMovies = this.typeFilter(this.allMovies)

                    this.isFindMovies = true
                    this.pageLength = Math.ceil(this.allMovies.length / this.perPage)
                    this.updatePage(1)
                }).catch(error => {
                    console.log(error)
                }).finally(() => {
                    this.isLoading = false
                })
        },
        typeFilter(movies) {
            if (this.type === '') {
                return movies
            }
            return movies.filter((movie) => movie.type === this.type)
        },
        updatePage(pageIndex) {
            this.movieList = this.allMovies.slice((pageIndex - 1) * this.perPage, pageIndex * this.perPage)
        },
    },
}
</script>
