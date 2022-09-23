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
                            outlined
                            dense
                            hide-details
                        ></v-text-field>
                    </v-col>
                    <v-col cols="2">
                        <v-text-field
                            label="年份（非必填）"
                            v-model="year"
                            outlined
                            dense
                            hide-details
                        ></v-text-field>
                    </v-col>
                    <v-col
                        class="d-flex"
                        cols="12"
                        sm="2"
                    >
                        <v-select
                            v-model="selected"
                            :items="types"
                            label="類型"
                            dense
                            solo
                            hide-details
                        ></v-select>
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
export default {
    name: 'SearchBar',
    data() {
        return {
            valid: false,
            keyword: '',
            year: '',
            typesTable: {
                所有類別: '',
                電影: 'movie', 
                影集: 'series',
                遊戲: 'game',
            },
            selected: '',
            types: [],
        }
    },
    created() {
        this.types = Object.keys(this.typesTable)
    },
    methods: {
        handleSearch() {
            const data = {
                keyword: this.keyword,
                year: this.year,
                type: this.typesTable[this.selected] || ''
            }
            this.$emit('handle-search', data)
        },
    },
    watch: {
        keyword() {
            this.valid = this.keyword.trim() !== ''
        }
    }
}
</script>