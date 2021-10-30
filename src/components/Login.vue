<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-4 mt-3">
                <!-- Html block for Login form -->
                <h4 class="text-center">Log In</h4>
                {{ status }}
                <form @submit.prevent="onLogin" enctype="multipart/form-data">
                    <div class="form-group mb-3">
                        <label for="login-email" class="label-form">Email Address:</label>
                        <input id="login-email" type="email" required class="form-control" placeholder="Enter Email Address" v-model="model.email">
                    </div>

                    <div class="form-group mb-3">
                        <label for="login-password" class="label-form">Password:</label>
                        <input id="login-password" type="password" required class="form-control" placeholder="Enter Password" v-model="model.password">
                    </div>

                    <div class="form-group text-center">
                        <button class="btn btn-success">Log In</button>
                        {{ loading }}
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
/* eslint-disable */
// eslint-disable-next-line

import axios from 'axios';

export default ({
    name: "Login",
    data() {
        return {
            model: {
                email: "",
                password: "",
            },
            loading: "",
            status: ""
        }
    },
    methods: {
        onLogin() {
            // const formData = new FormData();
            const self = this
            // formData.append("email", this.model.email);
            // formData.append("password", this.model.password);

            console.log('in login function')
            this.loading = "Logging In";
            // Post to server
            axios.post("http://10.30.30.26:9000/api/authentication/login", 
                {
                email: this.model.email,
                password: this.model.password},
                
                {headers: {'Content-Type':'multipart/form-data'}}
                ).then(res => {
                    //Post a status message
                    this.loading = "";
                    if (res.data.status == "success") {
                    // store the data in local storage
                    localStorage.setItem("token", res.data.data.token);

                    if (localStorage.getItem('token') != null){
                        self.$emit('loggedIn')
                        if(self.$route.params.nextUrl != null){
                        self.$router.push(this.$route.params.nextUrl)
                        }
                        else {
                            self.$router.push('/users')
                            }
                    }
                    } else {
                        this.loading = "";
                        this.status = res.data.message;
                    }
                }).catch( err =>{
                    this.status = "Wrong Email or Password. Please Try Again";
                });
            },
        },
})
</script>
