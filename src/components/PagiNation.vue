<template>
    <div class="text-center">
        <v-pagination
			class="pagination mb-2"
			v-model="page"
			:length="pageLength"
			@input="updatePage"
		></v-pagination>
    </div>
</template>

<script>
export default {
    name: 'PagiNation',
    props: {
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
    }
}
</script>
