<script setup>
import Swal from 'sweetalert2'
const supabase = useSupabaseClient()
const user = useSupabaseUser()
const dataview = ref()
const name = ref()

onMounted(async () => {
    try {
        const { data, error } = await supabase.auth.getSession(); // get session status from local cookies

        if (data.session.user.user_metadata.role == 'admin') {
            dataview.value = true
            name.value = data.session.user.identities[0].identity_data.first_name // Display registered username
            // dataview.value = data.session.user // JSON Body response
            // console.log(data.session.user.user_metadata.role)
            console.log('this user is admin')
        } else {
            navigateTo("/account")
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








// data
const InsertProduct = async () => {
    try {

        const { error } = await supabase
            .from('Products')
            .insert({ name: 'Bahrain', price: '20' })
        if (error) {
            console.error('Error inserting products:', error.message);
        } else {
            console.log('Success');
        }
    } catch (error) {
        console.error('Error inserting products:', error.message);
    }
};

//get data
const fetchProducts = async () => {
    try {
        const { data, error } = await supabase.from('Products').select('*');
        if (error) {
            console.error('Error fetching products:', error.message);
        } else {
            console.log('Products:', data);
        }
    } catch (error) {
        console.error('Error fetching products:', error.message);
    }
};

// delete data
const DeleteProduct = async () => {
    try {
        const { error } = await supabase
            .from('Products')
            .delete()
            .eq('name', 'Bahrain')
        if (error) {
            console.error('Error delete products:', error.message);
        } else {
            console.log('Success');
        }
    } catch (error) {
        console.error('Error delete products:', error.message);
    }
};

//update data
const UpdateProduct = async () => {
    try {
        const { error } = await supabase
            .from('Products')
            .update({ name: 'Australia' })
            .eq('id', 2)
        if (error) {
            console.error('Error update products:', error.message);
        } else {
            console.log('Success');
        }
    } catch (error) {
        console.error('Error update products:', error.message);
    }
};

// fetch image
const getimage = async () => {
    try {
        const { data, error } = supabase
            .storage
            .from('products_images')
            .getPublicUrl('Dreamliner-united..jpg')
        if (error) {
            console.error('Error:', error.message);
        } else {
            console.log('Success', data.publicUrl);
            Imagedata.value = data.publicUrl
            console.log(Imagedata.value);
        }
    } catch (error) {
        console.error('Error:', error.message);
    }
};

//upload image
const upimage = async () => {
    try {
        const file = event.target.files[0]
        const { data, error } = await supabase
            .storage
            .from('products_images')
            .upload(`${file.name}`, file, {
                cacheControl: '3600',
                upsert: false
            })

        console.log(file);
        if (error) {
            console.error('Error:', error.message);
        } else {
            console.log('Success', data.publicUrl);

        }
    } catch (error) {
        console.error('Error:', error.message);
    }
};

</script>
<template>
    <div v-if="dataview" class="mt-20 max-w-min p-10 justify-center   ">
        <!-- admin main page -->
        <!-- <button @click="makeAdmin">Make Admin</button> -->
        <p class="font-semibold text-3xl">Welcome, {{ name }}</p>
        <button @click="LogOut">Logout</button>
        <!-- <button @click="InsertProduct">INSERT</button>
        <button @click="fetchProducts">GET</button>
        <button @click="DeleteProduct">DELETE</button>
        <button @click="UpdateProduct">UPDATE</button>
        <button @click="getimage">GETIMAGE</button> -->
        <button @click="upimage">UPIMAGE</button>


        <div>
            <input type="file" accept="image/*" @change="upimage" />
        </div>
    </div>
</template>