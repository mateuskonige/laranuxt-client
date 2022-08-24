<template>
  <div class="container cols-md-6 mt-5">
    <nuxt-link class="btn btn-outline-primary" to="/products">Back</nuxt-link>
    <hr />

        <div v-if="$fetchState.pending">
            Carregando
        </div>


    <div v-else>
            <span># {{ product.id }}</span>
    <h2>{{ product.name }}</h2>

    <small>{{ product.created_at }}</small>
    <hr />
               <v-img
      width="250"
      :src="product.photo_url"
    ></v-img>

    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
    };
  },

  async fetch() {
    await this.getSingleProduct();
  },
  fetchDelay: 1000,

  methods: {
    async getSingleProduct() {
      const data = this.$axios.$get(`api/products/${this.$route.params.id}`);
      const res = await data;
      console.log(res.data);
      this.product = res.data;
    },
  },
};
</script>
