<template>
    <v-container>
        <v-data-table
            class="elevation-1"
            :headers="headers"
            :items="movies"
            :items-per-page="10"
        >
            <template v-slot:item="row">
                <tr>
                    <td>{{row.item.name}}</td>
                    <td>{{row.item.type}}</td>
                    <td>{{row.item.year}}</td>
                    <td>
                        <DetailDialog :id="row.item.imdbID" />
                    </td>
                </tr>
            </template>
        </v-data-table>
    </v-container>
</template>

<script>
import DetailDialog from './DetailDialog.vue'

export default {
    name: 'DataTable',
    components: {
        DetailDialog,
    },
    props: {
        movieList: {
            type: Array,
            default: () => [],
        },
    },
    data() {
        return {
            headers: [
                { text: '名稱', value: 'name' },
                { text: '類型', value: 'type' },
                { text: '年份', value: 'year' },
                { text: '', value: '' },
            ],
            movies: [],
        }
    },
    watch: {
        movieList() {
            this.movies = this.movieList.map((movie) => ({
                imdbID: movie.imdbID, 
                name: movie.Title,
                type: movie.Type,
                year: movie.Year,
            }))
        }
    }
}
</script>
