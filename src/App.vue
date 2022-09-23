<template>
    <v-app>
        <v-main>
            <SearchBar @handle-search="handleSearch"/>
            <DataTable
                v-if="isFindMovies"
                :movieList="movieList"
            />
            <LoadIng v-if="isLoading"/>
            <PagiNation
                v-if="isFindMovies"
                :initPage="page"
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
            keyword: '',
            isFindMovies: false,
            isLoading: false,
            page: 1,
            perPage: 10,
            pageLength: 0,
        }
    },
    methods: {
        handleSearch(value) {
            this.keyword = value[0].trim()
            this.type = value[1] || ''
            this.page = 1

            this.fetchMovies()
        },
        fetchMovies() {
            this.isLoading = true
            
            axios.get(`${API_URL}/?apikey=${API_KEY}&s=${this.keyword}&type=${this.type}&page=${this.page}`).
                then(response => {
                    this.pageLength = Math.ceil(response.data.totalResults / this.perPage)
                    const movies = response.data.Search

                    this.movieList = movies.map((movie) => ({
                        title: movie.Title,
                        type: movie.Type,
                        year: movie.Year,
                        id: movie.imdbID,
                    }))

                    this.isFindMovies = true
                }).catch(error => {
                    console.log(error)
                }).finally(() => {
                    this.isLoading = false
                })
        },
        updatePage(pageIndex) {
            this.page = pageIndex
            this.fetchMovies()
        },
    },
}
</script>
