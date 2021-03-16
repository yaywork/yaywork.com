<template>
    <div class="leading-5 mx-auto container max-w-lg">

        <h1 class="text-2xl lg:text-4xl font-black text-purple-900">Yay {{ $auth.user.username }}!</h1>
        <h2 class="lg:text-lg leading-3 mt-8 mb-20">
            You’re all signed up! Sadly there’s nothing to see here at the moment. But there will be soon!<br/><br/>
            In the meantime, maybe sign up for email alerts below?
        </h2>        

        <div class="mt-10">
            <span @click="logout" class="link">Logout --></span>
        </div>

    </div>
</template>
<script>
export default {
    middleware: ['auth', 'emailConfirmed'],    
    methods: {        
        async logout() {
            await this.$auth.logout();
        }
    },
}
</script>