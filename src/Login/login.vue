<template>
<div>
   <v-app>
     <main>
       <v-container fluid fill-height class="pt-5">
          <v-layout flex align-center justify-center>
            <v-flex xs12 sm4 elevation-6>
              <v-toolbar class="pt-5 blue darken-4">
                <v-toolbar-title class="white--text"><h4>Login</h4></v-toolbar-title>
                <v-toolbar-items/>
              </v-toolbar>
              <v-card>
                <v-card-text class="pt-4">
                  <div>
                      <v-form v-model="valid" ref="form">
                        <v-text-field
                          label="Enter your e-mail address"
                          v-model="email"
                          :rules="emailRules"
                          required
                        ></v-text-field>
                        <v-text-field
                          label="Enter your password"
                          v-model="password"
                          min="5"
                          :append-icon="e1 ? 'visibility_off' : 'visibility'"
                          :append-icon-cb="() => (e1 = !e1)"
                          :type="e1 ? 'password' : 'text'"
                          :rules="passwordRules"
                          counter
                          required
                        ></v-text-field>
                        <v-layout justify-space-between>
                            <v-btn @click="submit" :class=" { 'blue darken-4 white--text' : valid, disabled: !valid }">Login</v-btn>
                            <router-link to="register">Register</router-link>
                        </v-layout>
                      </v-form>
                  </div>
                </v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
       </v-container>
     </main>
   </v-app>
   </div>
</template>


<script>
import 'vuetify/dist/vuetify.min.css';
import axios from 'axios';

export default {

  data() {
    return {
      postBody: {},
      valid: false,
      e1: true,
      password: '',
      passwordRules: [
        v => !!v || 'Password is required',
      ],
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid',
      ],
    };
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        this.postBody = Object.assign({ Email: this.email, Password: this.password });
        axios.post('http://localhost:53653/json/reply/AuthenticateUser', this.postBody)
          .then((response) => {
            if(response.data != null){
            const userData = response.data;
            sessionStorage.setItem('user', userData.Email);
           // this.$store.dispatch('getTaskData');
           // this.$store.dispatch('getCategories');
            this.$router.push({ name: 'Home' });
            }
            else{
              alert("Username/Pasword Wrong")
            }
          })
          .catch((e) => {
            console.log(e);
            alert('Username/Password is wrong, Try Again!');
            this.clear();
          });
      }
    },
    clear() {
      this.$refs.form.reset();
    },
  },
};
</script>

<style>
#app {
  background-image: url("../assets/bg.jpg");
  background-size: cover;
  overflow:hidden;
}
.loginOverlay {
  background:rgba(33,150,243,0.3);
}
.photoCredit{
  position: absolute;
  bottom: 15px;
  right: 15px;
}
</style>
