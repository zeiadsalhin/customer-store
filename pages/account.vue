<script setup>
import Swal from 'sweetalert2'
const supabase = useSupabaseClient()
const user = useSupabaseUser()
const dataview = ref()
onMounted(async () => {
    try {
        const { data, error } = await supabase.auth.getSession(); // get session status from local cookies

        if (data.session.user.user_metadata.role == 'admin') {
            navigateTo("/admin")
            // name.value = data.session.user.identities[0].identity_data.first_name // Display registered username
            // dataview.value = data.session.user // JSON Body response
            // console.log(data.session.user.user_metadata.role)
            console.log('this user is admin')
        } else {
            dataview.value = true
            console.log('this user is regular')
        }
    } catch (error) {
        console.log(error);
    }

});


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
    <div v-if="dataview" class="mt-32">
        User page
        <button @click="LogOut">Logout</button>
    </div>
</template>