<template>
    <v-container>
        <v-data-table
            class="elevation-1"
            :headers="headers"
            :items="movieList"
            :items-per-page="10"
        >
            <template v-slot:item="row">
                <tr>
                    <td>{{row.item.Title}}</td>
                    <td>{{row.item.Type}}</td>
                    <td>{{row.item.Year}}</td>
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
        }
    },
}
</script>
