<template>
    <div>
        <v-card>
            <v-card-title>
                Products

                <v-spacer />

                <ProductsCreateModal @refetch="reFetch()"/>

            </v-card-title>

            <v-card-title>
                <v-text-field
                    outlined
                    small
                    width="200px"
                    type="text"
                    label="Search"
                    v-model="keyword"
                    prepend-inner-icon="mdi-magnify"
                    v-on:keyup.enter="searchProducts">
                </v-text-field>
            </v-card-title>
            <v-card-text>
                        <div v-if="loading" class="w-full">
                            <v-progress-linear
                                indeterminate
                                color="primary"
                            ></v-progress-linear>
                            <p class="text-center"><i>Carregando... Por favor, aguarde.</i></p>
                        </div>

                <v-simple-table>

                    <template v-slot:default v-if="products">
                        <thead>
                            <tr>
                                <th class="text-left">Photo</th>
                                <th class="text-left">Name</th>
                                <th class="text-left" width="5px">Ações</th>
                            </tr>
                        </thead>

                        <tbody>
                            <tr v-for="product in products" :key="product.name">
                                <td><v-avatar><img :src="product.photo_url" :alt="product.name"></v-avatar></td>
                                <td>{{ product.name }}</td>
                                <td>
                                                <v-menu
            bottom
            left
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>

              <v-list-item :to="{ name: 'products-id', params: { id: product.id } }">
                <v-list-item-icon>
                    <v-icon>mdi-eye</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                    <v-list-item-title >Detalhes</v-list-item-title>
                </v-list-item-content>
            </v-list-item>
                                                <ProductsEditModal :product="product" @refetch="reFetch()"/>
                                    <ProductsDeleteModal :product="product" @refetch="reFetch()"/>
            </v-list>

          </v-menu>



                                </td>
                            </tr>
                        </tbody>
                    </template>

                </v-simple-table>
            </v-card-text>

            <v-card-actions>
                <div class="text-center">
                    <v-pagination
                    @input="onPageChange"
                    v-model="pagination.current"
                    :length="pagination.total"
                    :total-visible="7"
                    circle
                ></v-pagination>
                </div>
            </v-card-actions>
        </v-card>
    </div>
</template>

<script>
  export default {
  data() {
    return {
        keyword: [],
      products: [],
      links: "",
      form: {},
       pagination: {
                current: 1,
                total: 0
            },
            dialog: false,
            loading: false,
    };
  },

  async fetch() {
    await this.getProducts();
  },
  fetchDelay: 1000,

  methods: {
    async searchProducts() {
            this.loading = true
            try {
                const data = this.$axios.$get(`api/products/search`, { params: { keyword: this.keyword } });
                let res = await data;
                console.log(res.data)
                this.products = res.data
                this.pagination.current = res.meta.current_page;
                this.pagination.total = res.meta.last_page;
                this.loading = false
            }
            catch (err) {
                console.log(err);
            }
        },
    async getProducts() {
            try {
                const data = this.$axios.$get(`api/products?page=${this.pagination.current}`);
                let res = await data;
                console.log(res.data)
                this.products = res.data
                this.pagination.current = res.meta.current_page;
                this.pagination.total = res.meta.last_page;
            }
            catch (err) {
                console.log(err);
            }
        },
        onPageChange() {
            this.$fetch();
        },

        reFetch() {
            this.$fetch();
        },
  },

};
</script>
