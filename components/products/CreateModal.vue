<template>
    <v-dialog
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">

        <v-btn
          color="primary"
            fab
            small
            elevation="0"
          v-bind="attrs"
          v-on="on"
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </template>
      <v-card>
            <form ref="form" @submit.prevent="store()" enctype="multipart/form-data">
        <v-card-title>
          <span class="text-h5">User Profile</span>
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
    data() {
        return {
            form: {
                name: "",
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

        async store() {

                const config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                }

                let data = new FormData();
                data.append('name', this.form.name);
                data.append('photo', this.form.photo);


                try {
                    await this.$axios.post('api/products', data, config)
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
