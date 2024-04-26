<script setup>
import Swal from 'sweetalert2'
const supabase = useSupabaseClient()
const user = useSupabaseUser()

onMounted(async () => {
    const user = useSupabaseUser()
    if (!user) {
        return navigateTo('/login');
    }

    // Check if the user is an admin by querying the user roles

    const { data, error } = await supabase.from('roles').select('*');

    console.log(data)

    if (error || !data || data.length === 0) {
        // return navigateTo('/not-authorized'); // Redirect to a "not-authorized" page
    }
})


async function LogOut() {
    try {
        const { error } = await supabase.auth.signOut()
        Swal.fire({
            title: 'Success',
            icon: 'success',
            text: 'You Signed out successfully',
            toast: true,
            timer: 1000,
            showConfirmButton: false,
        }).then(() => {
            navigateTo("/login")
        })
    } catch (error) {
        console.log(error)
    }
}
</script>
<template>
    <div class="mt-32">
        admin main page
        <!-- <button @click="makeAdmin">Make Admin</button> -->
        <button @click="LogOut">Logout</button>
    </div>
</template>