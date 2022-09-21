<template>
    <v-container>
        <v-data-table
            class="elevation-1"
            :headers="headers"
            :items="movieList"
            hide-default-footer
        >
            <template v-slot:item="row">
                <tr>
                    <td>{{row.item.title}}</td>
                    <td>{{row.item.type}}</td>
                    <td>{{row.item.year}}</td>
                    <td>
                        <DetailDialog :id="row.item.id" />
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

<style scoped>
tr:nth-child(odd) {
    background-color: #f5f5f5;
}
</style>
