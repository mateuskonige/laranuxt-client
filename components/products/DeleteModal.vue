<template>
        <v-dialog v-model="dialog" persistent max-width="600px">
            <template v-slot:activator="{ on, attrs }">

              <v-list-item v-bind="attrs" v-on="on">
                <v-list-item-icon>
                    <v-icon>mdi-delete</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    <v-list-item-title >Excluir</v-list-item-title>
                </v-list-item-content>
            </v-list-item>
            </template>
            <v-card>
                <v-card-title> Excluir </v-card-title>
                <v-card-text> excluir item? </v-card-text>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="dialog = false">
                        Close
                    </v-btn>
                    <v-btn color="error" @click="deleteProduct(product.id)">
                        Delete
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
</template>

<script>
export default {
    props: ["product"],
    data: () => ({
        dialog: false,
    }),

    methods: {
        async deleteProduct(id) {
            try {
                await this.$axios.$delete(`/api/products/${id}`);
                this.dialog = !this.dialog;
                this.$emit("refetch");
            } catch (err) {
                console.log(err);
            }
        },
    },
};
</script>
