<script setup>
const supabase = useSupabaseClient()
const user = useSupabaseUser()
const errMsg = ref()
const email = ref('')
const password = ref('')
const dataview = ref()

const signIn = async () => {
    const { error } = await supabase.auth.signInWithPassword({
        email: email.value,
        password: password.value,

    })
    if (error) console.log(error)
    errMsg.value = error
}

watch(user, () => {
    if (user.value) {
        // Redirect to protected page
        return navigateTo('/')
    } else {
        dataview.value = true
    }
}, { immediate: true })
</script>
<template>
    <div>
        <!--Login Form Body-->
        <!--will only render when no user exist-->
        <div v-if="dataview"
            class="p-1 md:p-5 md:my-20 mt-32 flex-col justify-center mx-auto w-11/12 h-fit shadow-2xl rounded-md">
            <h1 class="text-3xl md:text-5xl text-center font-bold p-5">Log in</h1>

            <div class="w-1/4 h-1 mt-5 rounded-xl mx-auto bg-gray-600 dark:bg-gray-900"></div>
            <form id="form" class="space-y-5 p-5 w-full h-fit text-center mx-auto justify-center flex-col"
                @submit.prevent="signIn">
                <!--input fields-->
                <div class="form mt-3 flex justify-center">
                    <label class="p-3 text-md text-center md:text-xl mr-7 hiddenm">Email</label>
                    <input placeholder="Email" v-model="email"
                        class="bg-gray-200 text-black p-1 md:p-2 ml- rounded-md focus:outline-none w-2/3 md:w-1/4 "
                        type="email" required />
                </div>
                <div class="form mt-3 flex justify-center">
                    <label class="p-3 text-md md:text-xl text-left md:-mr-1.5 hiddenm">Password</label>
                    <input placeholder="Password" v-model="password"
                        class="bg-gray-200 text-black p-1 md:p-2 rounded-md focus:outline-none w-2/3 md:w-1/4 "
                        type="password" required />
                </div>
                <!--Display error message if any-->
                <p class="text-red-500" v-if="errMsg">{{ errMsg }}</p>

                <!--Submit button-->
                <button @click="" type="submit"
                    class="px-5 m-5 py-2 w-32 rounded-md hover:cursor-pointer bg-gray-600 hover:bg-gray-500 hover:text-gray-50">
                    Log in
                </button>
                <!--Return to Sign Up page-->
                <NuxtLink to="/signup"><button
                        class="px-5 m-5 py-2 w-fit hover:cursor-pointer mx-auto block hover:underline hover:bg-gray-300">
                        return to Sign Up
                    </button></NuxtLink>
            </form>
        </div>
        <!--End of Body-->
    </div>
</template>