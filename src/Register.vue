<template>
<div >
<v-container>
<h1>Register your profile</h1>
<v-label for="nameInput">Name: </v-label><v-text-field type="text" required v-model="newUserName" ref="namePost" name="nameInput"></v-text-field>
<br>    
<v-label for="emailInput">Email: </v-label><v-text-field type="email" required v-model="newUserEmail" ref="emailPost" name="emailInput"></v-text-field>
<br>
<v-label for="passwordInput">Password: </v-label> <v-text-field type="password" required v-model="newUserPassword" ref="passwordPost" name ="passwordInput"></v-text-field>
<br>
<v-label for="cityInput">City(Optional): </v-label><v-text-field type="text" v-model="newUserCity" ref="cityPost" name="cityInput"></v-text-field>
<br>    
<v-label for="countryInput">Country(Optional): </v-label><v-text-field type="text" v-model="newUserCountry" ref="countryPost" name="countryInput"></v-text-field>
<br>
<!-- <label>Profile Picture(Optional, JPEG,PNG,GIF only): </label>  -->

<v-btn type="button" v-on:click="Register()">Register your profile</v-btn>
<v-btn type="button" v-on:click="toLogin()">Cancel</v-btn>
<br>
<label> {{ errorLabel }} </label>

</v-container>

</div>
</template>

<script>
export default {
        data: function (){
                return{
                    errorLabel: "",
                    errorFlag: false,
                    newUserName: "",
                    newUserEmail:"",
                    newUserPassword:"",
                    newUserCity:"",
                    newUserCountry:"",
                }
            },
    mounted: function() {
        this.checkLoggedStatus();
    },
    methods: { 
            Register: function() {
                this.$http.post('http://localhost:4941/api/v1/users/register', 
                {
                    // name :this.$refs.namePost.value,
                    name : this.newUserName,
                    email :this.newUserEmail,
                    password:this.newUserPassword,
                    city: this.newUserCity,
                    country:this.newUserCountry
                })
                .then((response) => {
                this.$cookies.set("userId", response.data.userId);
                this.$cookies.set("authToken", response.data.token);
                this.$router.push("/users/" + this.$cookies.get("userId"));


            })
            .catch((error) => {
                this.errorLabel = error;
                this.errorFlag = true;
            });
        },
        checkLoggedStatus: function() {
            if(this.$cookies.get("authToken")) {
                this.$router.push("/users/" + this.$cookies.get("userId"));
            }
        },
            toLogin: function() {
                this.$router.push("/userslogin/")
            }
    },
}
</script>