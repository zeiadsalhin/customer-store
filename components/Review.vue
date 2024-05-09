<script setup>

</script>
<template>
    <div class="p-4 mt-10 ">
        <h1 class="text-2xl mb-10">Write a Review</h1>
        <form class="space-y-2" @submit.prevent="submitReview">

            <div class="name flex space-x-5 p-2 md:w-1/2 mx-auto">
                <label for="name">Name:</label>
                <input type="text" id="name" class="w-full p-2 text-white bg-zinc-700" v-model="review.name" required>
            </div>

            <div class="review flex space-x-2 p-2 md:w-1/2 mx-auto">
                <label for="text">Review:</label>
                <textarea cols="10" rows="5" id="text" class="w-full p-2 text-white bg-zinc-700" v-model="review.text"
                    required></textarea>
            </div>

            <div class="rating flex p-2 md:w-1/2 mx-auto">
                <label for="rating">Rating:</label>
                <!-- <input type="number" id="rating"  min="1" max="5" required> -->
                <v-rating class="mx-auto flex justify-center bg-zinc-950 w-48" color="yellow" density="compact"
                    v-model="review.rating"></v-rating>
            </div>

            <button type="submit" class="w-48 flex justify-center mx-auto p-2 mt-10 bg-zinc-900 text-white">Submit
                Review</button>
        </form>
    </div>
</template>
<script>
import Swal from 'sweetalert2'
export default {
    data() {
        return {
            review: {
                name: '',
                text: '',
                rating: null,
            }
        };
    },
    methods: {
        async submitReview() {
            const supabase = useSupabaseClient();
            const productId = parseInt(this.$route.params.id);
            try {
                const { data, error } = await supabase.from('Reviews').insert({ post_id: productId, name: this.review.name, text: this.review.text, rating: this.review.rating });

                if (error) {
                    console.error(error);
                    // Handle error
                } else {
                    Swal.fire({
                        title: 'Success',
                        icon: 'success',
                        text: 'Review added successfully!',
                        toast: true,
                        timer: 2000,
                        showConfirmButton: false,
                    })

                    this.review = {
                        name: '',
                        text: '',
                        rating: null
                    };
                }
            } catch (error) {
                console.error('Error submitting review:', error.message);
                // Handle error
            }
        }
    }
};
</script>