<template>
    <div>
        <b-container>
            <h1 v-if="error">{{error}}</h1>
            <b-form-group
                id="input-group-1"
                label="Email address:"
                label-for="input-1"
                description="We'll never share your email with anyone else."
            >
                <b-form-input
                    id="email"
                    name="email"
                    v-model="form.email"
                    type="email"
                    placeholder="Enter email"
                    required
                ></b-form-input>
            </b-form-group>
            <label>Password</label>
            <b-form-input type="password" id="password" name="password" aria-describedby="password-help-block" v-model="form.password"></b-form-input>
            <b-button variant="primary" @click.prevent="login">Login</b-button>
        </b-container>

    </div>
</template>

<script>
export default {
    name: "Login",
    data() {
        return {
            error: null,
            form:{
                email: null,
                password: null

            }
        }
    },
    methods: {
        login() {
            this.error = null;
            axios.get('/sanctum/csrf-cookie').then(res => {
                console.log('res',res)
                axios.post('/login',{email: this.form.email, password: this.form.password})
                    .then(result => {
                        console.log('result',result.config.headers['X-XSRF-TOKEN'])
                        localStorage.setItem('x_xsrf_token',result.config.headers['X-XSRF-TOKEN'])
                        this.$router.push({name: 'home'})
                    }).catch(error => {
                    console.log('Error:', error.response.data)
                    this.error = error.response.data.error
                })
            }).catch(e => {
                console.log('ERROR: ',e)
                this.error = e.response.data.error
            })

        }
    }
}
</script>

<style scoped>

</style>
