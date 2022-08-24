<template>
    <v-row justify="center" align="center">
        <v-col cols="12" sm="8" md="6">
            <v-card class="logo py-4 d-flex justify-center">
                {{ response }}
                {{ $auth.user }}
            </v-card>
        </v-col>
    </v-row>
</template>

<script>
export default {
    data() {
        return {
            response: "",
        };
    },

    fetch() {
        return this.getData();
    },

    methods: {
        async getData() {
            await this.$axios
                .$get("api/")
                .then((res) => {
                    const data = res;
                    this.response = data;
                })
                .catch((err) => {
                    console.log(err);
                });
        },
    },
};
</script>
