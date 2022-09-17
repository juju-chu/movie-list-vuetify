<template>
    <div>
        <v-form>
            <v-container>
                <v-row
                    align="center"
                    justify="center"
                >
                    <v-col cols="12" sm="3">
                        <v-text-field
                            label="請輸入電影名稱"
                            v-model="keyword"
                            required
                        ></v-text-field>
                    </v-col>
                    <v-btn
                        class="mr-4"
                        :disabled="!valid"
                        color="info"
                        @click="handleSearch"
                    >
                        Search
                    </v-btn>
                </v-row>
            </v-container>
        </v-form>
    </div>
</template>

<script>
import axios from 'axios'
import { API_I, API_KEY, API_URL } from '../common/constants'

export default {
    name: 'SearchBar',
    data() {
        return {
            valid: false,
            keyword: '',
            movieList: [],
        }
    },
    methods: {
        handleSearch() {
            this.fetchMovies()
        },
        fetchMovies() {
            axios.get(`${API_URL}/?i=${API_I}&apikey=${API_KEY}&s=${this.keyword.trim()}`).
                then(response => {
                    this.movieList = response.data.Search
                }).catch(error => {
                    console.log(error)
                })
        }
    },
    watch: {
        keyword() {
            this.valid = this.keyword.trim() !== ''
        }
    }
}
</script>