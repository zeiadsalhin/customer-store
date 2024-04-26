<script setup>
import { useMainStore } from '@/store';

const mainStore = useMainStore();
const cartItems = computed(() => mainStore.items);
</script>
<template>
    <div>
        <v-app-bar color="gray" :elevation="2" app>
            <!-- <v-toolbar-title class="md:ml-10"> -->
            <nuxt-link to="/" class="md:ml-5">
                <v-icon size="30" class="p-6">mdi-home</v-icon>
            </nuxt-link>
            <nuxt-link to="/">
                <v-toolbar-title class="mr-10">
                    <h1 class="font-semibold text-xl">E-commerce</h1>
                </v-toolbar-title>
            </nuxt-link>
            <!-- </v-toolbar-title> -->
            <v-spacer />
            <nuxt-link to="/admin">
                <v-btn class="mr-md-2" icon>
                    <v-icon size="20">mdi-account-outline</v-icon></v-btn>
            </nuxt-link>
            <nuxt-link to="/products"><v-btn class="mr-md-2" icon>
                    <v-icon size="20">mdi-store-outline</v-icon>
                </v-btn></nuxt-link>
            <v-badge v-if="cartItems.length > 0" :content="cartItems.length">
                <v-btn nuxt to="/cart" icon>
                    <v-icon size="20">mdi-cart-outline</v-icon>
                </v-btn>
            </v-badge>
            <v-btn v-else nuxt to="/cart" icon>
                <v-icon size="20">mdi-cart-outline</v-icon>
            </v-btn>
            <!-- <v-divider vertical class="w-2" /> -->
            <v-btn @click="toggleTheme" icon>
                <v-icon size="20">mdi-brightness-7</v-icon>
            </v-btn>
        </v-app-bar>
    </div>
</template>
<script>
export default {
    methods: {
        toggleTheme() {
            // theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark';
            localStorage.scheme === 'light' ? localStorage.scheme = 'dark' : localStorage.scheme = 'light';
            this.$router.go()
        },
    },
};
</script>