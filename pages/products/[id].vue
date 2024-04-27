<script setup>
import Swal from 'sweetalert2'
import { useMainStore } from '@/store';

const mainStore = useMainStore();

const addToCart = (product) => {
    mainStore.addToCart(product);

};
</script>
<template>
    <div>
        <v-container v-if="product" class="mt-20">
            <v-row justify="center">
                <v-col cols="11" md="7">
                    <h2 class="text-center text-h4 font-weight-bold">
                        {{ product.name }}
                    </h2>
                    <div class="mt-2 text-center">
                        <v-rating readonly half-increments class="p-5" color="yellow darken-2"
                            background-color="grey lighten-1" :value="product.ratings" dense size="20"></v-rating>
                        <v-chip small label outlined class="mr-1" v-for="(t, i) in product.tags"
                            :key="`prod${product.id}-${i}`">
                            {{ t }}
                        </v-chip>
                    </div>
                    <br />
                    <v-img width="100%" class="el rounded-lg" height="50vh" :src="product.image"></v-img>
                    <p class="mt-5 mb-5">
                        {{ product.description }}
                    </p>
                    <p class="text-h5 mb-7">
                        Price: {{ product.price + ' $' }}
                    </p>
                    <v-btn @click="addToCart(product)" min-height="45" min-width="150" class="m-2" color="primary">Add
                        To Cart</v-btn>
                    <v-btn @click="addToCart(product), navigateTo('/checkout')" min-height="45" min-width="120"
                        class="m-2" color="lighten-gray">Buy Now</v-btn>
                </v-col>
            </v-row>
        </v-container>
        <br /><br />
        <Footer />
        <ScrollTop />
    </div>
</template>

<script>
export default {
    data() {
        return {
            product: null,
        };
    },
    mounted() {
        this.fetchProducts();
    },
    methods: {

        async fetchProducts() {
            const supabase = useSupabaseClient()
            const user = useSupabaseUser()
            const productId = parseInt(this.$route.params.id);
            try {

                const { data, error } = await supabase
                    .from('Products')
                    .select('*')
                    .eq('id', `${productId}`)

                this.product = data[0]

            } catch (error) {
                console.error('Error fetching products:', error.message);
            }
        },

    },
};
</script>

<style></style>