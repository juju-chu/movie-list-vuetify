<template>
    <v-col cols="auto">
        <v-dialog transition="dialog-top-transition" max-width="600">
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                    class="white--text"
                    color="grey darken-1"
                    v-bind="attrs"
                    v-on="on"
                    @click="handleClick"
                >
                    詳細資料
                </v-btn>
            </template>
            <template v-slot:default="dialog">
                <LoadIng v-if="isLoading" />
                <v-card v-else>
                    <v-toolbar
                        class="text-h5"
                        height="100%"
                        color="primary"
                        dark
                    >
                        {{ details.title }}
                    </v-toolbar>
                    <v-card-text>
                        <div class="text-body-1 pa-12">
                            <ul>
                                <li><span class="font-weight-bold">演員:</span> {{ details.actors }}</li>
                                <li><span class="font-weight-bold">類型:</span> {{ details.type }}</li>
                                <li><span class="font-weight-bold">故事內容簡介:</span> {{ details.plot }}</li>
                            </ul>
                        </div>
                    </v-card-text>
                    <v-card-actions class="justify-end">
                        <v-btn
                            text
                            @click="dialog.value = false"
                        >
                            Close
                        </v-btn>
                    </v-card-actions>
                </v-card>
            </template>
        </v-dialog>
    </v-col>
</template>

<script>
import axios from 'axios'
import { API_KEY, API_URL } from '../common/constants'
import LoadIng from './LoadIng'

export default {
    name: 'DetailDialog',
    components: {
        LoadIng,
    },
    props: {
        id: {
            type: String,
            default: '',
        }
    },
    data() {
        return {
            vOn: false,
            details: {},
            isLoading: false,
        }
    },
    methods: {
        fetchMovieDetail(id) {
            this.isLoading = true
            axios.get(`${API_URL}/?apikey=${API_KEY}&i=${id}`).
                then(response => {
                    this.movieDetail = response.data
                    this.details = {
                        title: this.movieDetail.Title,
                        actors: this.movieDetail.Actors,
                        type: this.movieDetail.Type,
                        plot: this.movieDetail.Plot,
                    }
                }).catch(error => {
                    console.log(error)
                }).finally(() => {
                    this.isLoading = false
                })
        },
        handleClick() {
            this.fetchMovieDetail(this.id)
        },
    }
}
</script>
