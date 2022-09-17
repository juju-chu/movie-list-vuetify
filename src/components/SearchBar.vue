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
                            v-model="name"
                            required
                        ></v-text-field>
                    </v-col>
                    <v-btn
                        class="mr-4"
                        :disabled="!valid"
                        color="info"
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
            name: '',
        }
    },
    watch: {
        name() {
            this.valid = this.name.trim() !== '';
        }
    }
}
</script>