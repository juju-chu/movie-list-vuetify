<template>
    <v-col cols="auto">
        <v-dialog transition="dialog-top-transition" max-width="600">
            <template v-slot:activator="{ on, attrs }">
                <v-btn color="primary" v-bind="attrs" v-on="on" @click="handleClick">詳細資料</v-btn>
            </template>
            <template v-slot:default="dialog">
                <v-card>
                    <v-toolbar class="text-h4" color="primary" dark>{{ title }}</v-toolbar>
                    <v-card-text>
                        <div class="text-body-1 pa-12">
                            <ul>
                                <li>演員: {{ actors }}</li>
                                <li>類型: {{ type }}</li>
                                <li>故事內容簡介: {{ plot }}</li>
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
            title: '',
            actors: '',
            type: '',
            plot: '',
        }
    },
    methods: {
        fetchMovieDetail(id) {
            axios.get(`${API_URL}/?apikey=${API_KEY}&i=${id}`).
                then(response => {
                    this.movieDetail = response.data
                    this.title = this.movieDetail.Title
                    this.actors = this.movieDetail.Actors
                    this.type = this.movieDetail.Type
                    this.plot = this.movieDetail.Plot
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
