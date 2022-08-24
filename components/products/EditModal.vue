<template>
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">

              <v-list-item v-bind="attrs" v-on="on">
                <v-list-item-icon>
                    <v-icon>mdi-pencil</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    <v-list-item-title >Editar</v-list-item-title>
                </v-list-item-content>
            </v-list-item>
      </template>
      <v-card>
            <form ref="form" @submit.prevent="update()" enctype="multipart/form-data">
        <v-card-title>
          <span class="text-h5">Update product</span>
        </v-card-title>
        <v-card-text>
          <v-container>
                <v-text-field
                    outlined
                    type="text"
                    label="Name"
                    v-model="form.name"
                    :error-messages="errors.name">
                </v-text-field>


                <input
                    type="file"
                    ref="file"
                    @change="handleFileObject()"/>
                <small v-if="errors.photo">{{errors.photo}}</small>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
          type="submit"
            color="primary"

          >
            Save
          </v-btn>
        </v-card-actions>
                  </form>
      </v-card>

    </v-dialog>
</template>

 <script>
export default {
    props: ['product'],

    data() {
        return {
            form: {
                name: this.product.name,
                photo: null,
            },
            errors: [],
            dialog: false,
        };
    },

    methods: {
        handleFileObject() {
                this.form.photo = this.$refs.file.files[0];
        },

        async update() {
                const config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                }

                let data = new FormData();
                data.append('name', this.form.name);
                if(this.form.photo != null){
                    data.append('photo', this.form.photo);
                }
                data.append('_method', 'PUT')

                try {
                    await this.$axios.post(`api/products/${this.product.id}`, data, config)
                            this.dialog = !this.dialog;
                            this.$emit('refetch')
                } catch (err) {
                    this.errors = [];
                        if ((err.response.status == 422)) {
                            this.errors = err.response.data.errors;
                            console.log(this.errors);
                        }
                }
        },

    },
};
</script>
