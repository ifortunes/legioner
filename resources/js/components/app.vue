<template>
    <v-container>
        <v-col
            class="d-flex"
            cols="12"
            sm="6"
        >
            <v-select
                v-model="sorting"
                :items="items"
                label="Сортировка"
                single-line
            ></v-select>
        </v-col>
        <v-row>
            <v-col
                cols="4"
                v-for="(item, i) in ARTICLES"
                :key="i"
            >
                <v-card
                    class="mx-auto"
                    max-width="344"
                >
                    <v-img
                        :src="item.image"
                        height="200px"
                    ></v-img>

                    <v-card-title>
                        {{ item.name }}
                    </v-card-title>
                </v-card>

            </v-col>
        </v-row>
        <div class="text-center">
            <v-pagination
                v-model="page"
                :length="LAST_PAGE"
                circle
            ></v-pagination>
        </div>
    </v-container>
</template>

<script>
import {mapActions, mapGetters} from "vuex";

export default {
    name: "app",
    data: () => ({
        page: 1,
        sorting: 'desc=price',
        items: [
            { text: 'По цене(убывание)', value: 'desc=price' },
            { text: 'По цене(возрастание)', value: 'asc=price' },
            { text: 'По дате создания(убывание)', value: 'desc=created_at' },
            { text: 'По дате создания(возрастание)', value: 'asc=created_at' },
        ]
    }),
    computed: mapGetters(['ARTICLES', 'LAST_PAGE']),
    methods: {
        ...mapActions([
            'GET_ARTICLES',
        ]),

        getArticles(){
            this.GET_ARTICLES({
                page: this.page,
                sorting: '&' + this.sorting
            })
        }
    },
    watch: {
        page(){
            this.getArticles()
        },

        sorting(){
            this.getArticles()
        }
    },
    created() {
        this.getArticles()
    }
}
</script>

<style scoped>

</style>
