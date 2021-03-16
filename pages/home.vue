<template>
    <div class="leading-5 mx-auto container max-w-lg">

        <h1>Woohoo! You’re signed up!</h1>
        <h2>Sadly there’s nothing to see here at the moment. But there will be soon!</h2>

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