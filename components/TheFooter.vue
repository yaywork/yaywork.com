<template>
    <div class="mt-20 mb-8 max-w-3xl mx-auto">
        <div>
            <div class="bg-lime-200 inline-block p-4">
                <h3 class="font-black uppercase italic">FOLLOW ALONG</h3>
                <p class="mt-2 leading-3 text-lime-900">Yay Work is being built in public. <a href="https://twitter.com/YayWork" target="_blank">Click here</a> to see daily updates.</p>
            </div>
        </div>
        
        <div class="bg-amber-300 max-w-md p-6 mx-auto mt-4">
            <h3 class="font-black uppercase italic">Subscribe</h3>
            <p class="mt-2 leading-3 text-amber-900">Drop your email and I’ll email you when interesting things happen.</p>
            <form method="post" :action="'https://sendfox.com/form/' + sendfox_user_id + '/' + sendfox_form_id " class="sendfox-form mt-2" :id="sendfox_form_id" data-async="true" data-recaptcha="false">            
                <input type="email" placeholder="very@cool.com" name="email" required :value="[$auth.loggedIn ? $auth.user.email : '']" class="h-8 px-4"/>
                <button native-type="submit" class="bg-rose-600 h-8 px-4 ml-4 hover:bg-blueGray-900 hover:text-white mt-1"><v-icon name="long-arrow-alt-right" scale="2.0"/></button>
                <div style="position: absolute; left: -5000px;" aria-hidden="true"><input type="text" name="a_password" tabindex="-1" value="" autocomplete="off" /></div>          
            </form>
            <script src="https://sendfox.com/js/form.js"></script>   
        </div>

        <div class="bg-rose-300 ml-auto max-w-2xl mt-4 p-2">
            <h3 class="font-black uppercase italic">Say Hello</h3>
            <p class="mt-2 leading-3 text-rose-900">Want to help? Have questions about joining? Or just want to talk about religion and politics? Send a message to <a href="mailto:pat@yaywork.com">pat@yaywork.com</a>.<br/><br/>Ok thx bye.</p>
        </div>


        <div class="mt-40 text-center text-blueGray-600 leading-3">
            &copy;Copyright 2021 Yay Work
            
            <template v-if="true">
              <!-- <span class="mx-4 hidden lg:inline">-</span>
              <br class="lg:hidden"/>

              <nuxt-link to="/terms">Terms</nuxt-link> and <nuxt-link to="/privacy">Privacy</nuxt-link> 
              -->
              <span class="mx-2 hidden lg:inline">-</span>
              <br class="lg:hidden"/> 
              <a href="https://app.splitbee.io/public/yaywork.com" target="_blank">Analytics</a>

              <span class="mx-2 hidden lg:inline">-</span>
              <br class="lg:hidden"/> 
              <a href="https://github.com/yaywork" target="_blank">GitHub</a>
            </template>
        </div>
 
    </div>
</template>

<script>
export default {
  computed: {
    sendfox_user_id() {
      return process.env.SENDFOX_USER_ID
    },
    sendfox_form_id() {
      return process.env.SENDFOX_FORM_ID
    }
  }
}
</script>