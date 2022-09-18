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
                        <v-btn
                            class="mx-2"
                            @click="onButtonClick(row.item)"
                        >
                            詳細資料
                        </v-btn>
                    </td>
                </tr>
            </template>
        </v-data-table>
    </v-container>
</template>

<script>
export default {
    name: 'DataTable',
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
    methods: {
        onButtonClick(item) {
            // TODO: 點擊彈出 Pop up 顯示該電影的詳細資料
            console.log('click on ' + item.imdbID)
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
