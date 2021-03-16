<!-- Okay, let's get started! -->

<!-- 
How do you think you'll primarily use yaywork?
[as a freelancer] [as part of an agency] [Some of each] [I'm not sure yet]
(don't worry, you can change this later)

if agency -> okay awesome, we'll get to your agency in a minute. But first, how should we refer to you?

[first name or preferred nickname] is super awesome. [His/her/their/other] work is incredible and I love working with [him/her/them/other]. [He/she/they/other] makes my heart happy.
-->

<!-- Sweet. Hi Patrick. That's a lovely name :) -->

<!-- Now, what do you want? Please rate the truth of each statement, which will help tailor your experience. (don't worry, you can change this later. ->
<!--
I want more client leads.
[Eh, not really.] [It couldn't hurt.] [Yes please.] [OMG you have no idea.]

I want to turn more leads into clients.

I want to get more repeat and referral business.

I want to stop doing so much paperwork (1099, NDA, scope of work, proposals, etc)

I want better communication with clients.

I want to get paid faster, easier, 

-->

<!-- Now let's get you authenticated. Cuz you know, security and stuff. -->

<!-- Create account with Google, Facebook, GitHub, or with email magic link. Just Google and GitHub would be sufficient for dev crowd, but we want to attract freelancers of all types, so it's important to have Facebook in there. --> 

<!-- Hooray, it worked! Now, would you like to change your username? People will able to find you at yaywork.com/patrick-lorenzut-griffith. -->
<template>
    <div class="container">      
        <form id="signup-form" @submit.prevent="moveAlong">
            
            <div v-show="step === 0">
                <h1>Let’s get this <br />work party started</h1>                    
                <div>
                    <label class="label">First things first... what’s your name?</label>                    
                    <div><input class="input w-full" ref="name" type="text" v-validate="'required|max:191'" name="name" placeholder="Full Name" v-model="newUser.name" /></div>
                    <div class="validate-error" v-if="errors.first('name')">{{ errors.first('name') }}</div>
                    <div class="mt-10 ml-2">
                        <button class="button" native-type="submit">Next --></button>
                    </div>

                    <p class="mt-10 ml-2 article">
                        Alread have an account? <nuxt-link to="/auth/login">Login.</nuxt-link>
                    </p>
                </div>
            </div>
            <div v-show="step === 1">
                <h1>Whoah, that’s a cool name. <br />Props to your parents.</h1>
                <div>
                    <label class="label">Now let’s choose a username to go with it.</label>                    
                    <div class="bg-white pl-4 inline-block">
                        yaywork.com/
                        <input class="input" ref="username" v-validate="'required|alpha_dash|max:15'" type="text" name="username" placeholder="username" v-model="newUser.username" />
                    </div>
                    <div class="validate-error" v-if="errors.first('username')">{{ errors.first('username') }}</div>
                    <div class="mt-10 ml-2">
                        <button class="button" native-type="submit"><span class="load"></span>Next --></button>
                    </div>
                </div>
            </div>
            <div v-show="step === 2">
                <h1>Last step <br/></h1>
                <div>
                    <label class="label">Email Address</label>                    
                    <div><input class="input w-full" ref="email" type="email" v-validate="'required|email|max:191'" name="email" placeholder="name@email.com" v-model="newUser.email" /></div>
                    <div class="validate-error" v-if="errors.first('email')">{{ errors.first('email') }}</div>

                    <label class="label">Password</label>
                    <div><input class="input w-full" type="password" v-validate="'required|min:6|max:191'" name="password" ref="password" placeholder="********" v-model="newUser.password" /></div>
                    <div class="validate-error" v-if="errors.first('password')">{{ errors.first('password') }}</div>

                    <label class="label">Confirm Password</label>
                    <div><input class="input w-full" type="password" v-validate="'required|confirmed:password|min:6|max:191'" name="password_confirmation" placeholder="********" v-model="newUser.password_confirmation" /></div>
                    <div class="validate-error" v-if="errors.first('password_confirmation')">{{ errors.first('password_confirmation') }}</div>

                    <div class="text-blue-600 text-xs mt-4">
                        <label>
                            <input class="input inline-block align-middle" type="checkbox" name="terms" v-validate="'required: { allowFalse: false }'" /> 
                            <span class="inline-block align-middle">Yes, I agree to the terms of service and privacy policy.</span>                        
                        </label>
                        <div class="validate-error" v-if="errors.first('terms')">{{ errors.first('terms') }}</div>
                    </div>

                    <div class="mt-10 ml-2">
                        <button class="button" native-type="submit"><span class="load"></span>Create Account --></button>
                    </div>
                </div>
            </div>
            
        </form>
    </div>
</template>
<script>
export default {
    layout: 'auth',
    middleware: 'auth',
    auth: 'guest',
    data() {
        return {
            step: 0,
            newUser: {
                name:  '',
                username: '',
                email: '',
                password: '',
                password_confirmation: '',
            }
        }
    },
    head() {
        return {      
            title: 'Start Yay Working',            
        }
    },
    methods: {
        async login() {
            try {
                await this.$auth.loginWith('laravelJWT', {
                  data: {
                    email: this.newUser.email,
                    password: this.newUser.password
                  }
                })
                this.$store.dispatch('stopLoading')
            } catch (e) { 
                this.$catchError(e)
            }
        },
        async signup() {
            try {
                this.$store.dispatch('startLoading')
                let userResponse = await this.$axios.post('/user', this.newUser)
                this.login()
            } catch (e) { 
                this.$catchError(e)
            }   
        },
        async checkUsername() {
            try {
                this.$store.dispatch('startLoading')
                let usernameResponse = await this.$axios.get('/user/checkUsername/' + this.newUser.username)
                this.$store.dispatch('stopLoading')
                if(usernameResponse.data.available) {
                    this.step = 2
                    this.$nextTick(() => {
                        this.$refs.email.focus()
                    })
                } else {
                    this.$toast.error('Sorry, that username is already taken.')
                }
            } catch (e) { 
                this.$catchError(e)
            }   
        },
        moveAlong() {
            if(!this.$store.state.isLoading) {
                if(this.step === 0) {
                    this.$validator.validate('name').then((result) => {
                        if (result) {
                            this.step = 1
                            this.$nextTick(() => {
                                this.$refs.username.focus()
                            })
                        }
                    })                
                } else if(this.step === 1) {
                    this.$validator.validate('username').then((result) => {
                        if(result) {
                            this.checkUsername()
                        }
                    })
                } else {
                    this.$validator.validateAll().then((result) => {
                        if(result) {
                            this.signup()
                        }
                    })
                }
            }
        },
    },
    async mounted() {
        this.$refs.name.focus()        
    }
}
</script>
<style lang="scss" scoped>
#signup-form {
    h1 {
        @apply pt-4 max-w-3xl mx-auto text-center mb-10 leading-1 text-blue-800 text-3xl;
        @media screen and (min-width: theme('screens.lg')) {
            @apply text-4xl;
        }
        br {
            display: none;
            @media screen and (min-width: theme('screens.md')) {
                display: block;
            }
        }
    }
    > div > div {
        @apply mx-auto max-w-sm;
    }
}
</style>