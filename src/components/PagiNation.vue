<template>
    <div class="text-center">
        <v-pagination
			class="pagination mb-2"
			v-model="page"
			:length="pageLength"
            :total-visible="7"
			@input="updatePage"
		></v-pagination>
    </div>
</template>

<script>
export default {
    name: 'PagiNation',
    props: {
        initPage: {
            type: Number,
            required: true,
        },
        pageLength: {
            type: Number,
            required: true,
        },
    },
    data() {
        return {
            page: 1,
        };
    },
    methods: {
        updatePage(pageIndex) {
            this.$emit('update-page', pageIndex)
        }
    },
    watch: {
        initPage() {
            this.page = this.initPage
        }
    }
}
</script>
