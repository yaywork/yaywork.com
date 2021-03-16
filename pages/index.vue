<template>
  <div class="mx-auto max-w-xl mt-10 mb-40">
    
    <h1 class="text-2xl lg:text-4xl font-black text-purple-900"><span class="">Independent Together</span></h1>
    <h2 class="lg:text-lg leading-3 mt-8 mb-20">            
        Yay Work is a new way for independent creators to work. It's sorta like Twitter, Jira, Kickstarter, and Upwork all rolled into one.<br/><br/>
        You’ve heard of #BuildInPublic. Now it's time to manage, sell, and get hired in public.
    </h2>

    <div class="mt-8">
      <nuxt-link to="/auth/start" class="button">Reserve your username --></nuxt-link>
    </div>
    
  </div>
</template>
<script>
export default {
  mounted() {
    if(this.$route.query.subscribed) {
      this.$toast.success('Thanks! Now please check your email :)')
    }
  }
}
</script>
