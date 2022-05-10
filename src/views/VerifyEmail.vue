<template>
  <div class='flex flex-col justify-center py-6 min-h-screen bg-gray-50 sm:px-6 lg:px-8'>
    <Heading />
    <div class='mt-8 sm:mx-auto sm:w-full sm:max-w-md'>
      <div class='px-4 py-8 bg-white shadow-xl sm:rounded-lg sm:px-10'>
        <h2 class='mt-3 mb-12 text-2xl font-bold text-center text-second-color-600'>
          {{ $t('Verify your email') }}
        </h2>
        <form class='space-y-6' @submit.prevent='this.verifyEmail'>
          <div class='mt-1'>
            <input id='email' :placeholder='$t("Email")'
                   v-model="email"
                   autocomplete='email'
                   class='block px-3 py-2 w-full placeholder-gray-400 rounded-md border border-gray-300 shadow-sm appearance-none focus:outline-none focus:ring-main-color-500 focus:border-main-color-500 sm:text-sm'
                   name='email'
                   required='required'
                   type='email' />
          </div>
          <div class='mt-1'>
            <input id='code' :placeholder='$t("Verification Code")'
                   v-model="code"
                   autocomplete='code'
                   class='block px-3 py-2 w-full placeholder-gray-400 rounded-md border border-gray-300 shadow-sm appearance-none focus:outline-none focus:ring-main-color-500 focus:border-main-color-500 sm:text-sm'
                   name='code'
                   required='required'
                   type='text' />
          </div>

          <button
            class='flex justify-center px-4 py-2 w-full text-sm font-medium text-white rounded-md border border-transparent shadow-sm bg-main-color-600 hover:bg-main-color-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-main-color-500'
            type='submit'>
            {{ $t('Sign Up') }}
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import Heading from '../components/auth/heading.vue'
import SocialLogin from '../components/auth/social_login.vue'
import axios from 'axios';

export default {
    components: {
      SocialLogin,
      Heading,
    },
    data (){
        return {
            email : '',
            code : '',
        }
    },
    mounted(){
      this.email = this.$route.params.email
    },
    methods : {
        verifyEmail(e){
            e.preventDefault();
            let self = this
            axios.post('https://admin.allsolutioncompany.com/api/verify-email-otp', {
              email:this.email,
              code:this.code,
            })
            .then(function (response) {
              console.log(response);
              if(response.data.status==true){
                self.$router.push({name:'Login'})
              }else{
                alert('Invalid verification code. Please try again')
              }
            })
            .catch(function (error) {
              console.log(error);
            });

        }
    }
  // computed: {
  //   ...mapState('user', {
  //     user: state => state.user,
  //   }),
  // },
  // methods: {
  //   ...mapActions('user', ['registerAction']),
  //   ...mapActions('snackbar', ['toggleSnackBarAction']),

  //   newRegister(event){
  //     event.preventDefault();
  //     alert(this.event);
  //     console.log(event);
  //   },

  //   register(event) {
  //     this.registerAction(event).then(result => {
  //       if (result.type === 'success') {
          
  //         this.$router.push({name:'Login'})
  //         // this.$router.push({ name: 'Home' })
  //       } else {
  //         this.toggleSnackBarAction(result)
  //       }
  //     })
  //   },
  // },
}
</script>