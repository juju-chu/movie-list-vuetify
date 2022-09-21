<template>
    <v-col cols="auto">
        <v-dialog transition="dialog-top-transition" max-width="600">
            <template v-slot:activator="{ on, attrs }">
                <v-btn color="primary" v-bind="attrs" v-on="on" @click="handleClick">詳細資料</v-btn>
            </template>
            <template v-slot:default="dialog">
                <v-card>
                    <v-toolbar class="text-h4" color="primary" dark>{{ details.title }}</v-toolbar>
                    <v-card-text>
                        <div class="text-body-1 pa-12">
                            <ul>
                                <li>演員: {{ details.actors }}</li>
                                <li>類型: {{ details.type }}</li>
                                <li>故事內容簡介: {{ details.plot }}</li>
                            </ul>
                        </div>
                    </v-card-text>
                    <v-card-actions class="justify-end">
                        <v-btn text @click="dialog.value = false">Close</v-btn>
                    </v-card-actions>
                </v-card>
            </template>
        </v-dialog>
    </v-col>
</template>

<script>
import axios from 'axios'
import { API_KEY, API_URL } from '../common/constants'

export default {
    name: 'DetailDialog',
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
        }
    },
    methods: {
        fetchMovieDetail(id) {
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
                })
        },
        handleClick() {
            this.fetchMovieDetail(this.id)
        },
    }
}
</script>
