<template>
    <div id="login">
        <h1>Login</h1>
        <div class="form-inputs">
            <label for="username">Username</label>
            <input type="text" id="username" name="username" v-model="input.username" placeholder="Username" />
        </div>
        <div class="form-inputs">
            <label for="password">Password</label>
            <input type="password" id="password" name="password" v-model="input.password" placeholder="Password" />
        </div>
        <button type="button" v-on:click="login()">Login</button>
        <div v-if="set">
            <p > user data</p>
            <ul id="example-2">
        <li v-for="(item, index) in vals">
             amount of times completed {{index}} : {{ item.length }}
             <br />
<br />average  {{index}} completion time: {{

    item.reduce(function (accumVariable, curValue) {
return accumVariable + curValue[3]
}, 0)
}}
        </li>
        </ul>
        
        </div>
            


    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: 'Login',
        data() {
            return {
                input: {
                    username: "",
                    password: ""
                },
                set:false,
                vals:{}

            }
        },
        methods: {
            login() {
                if(this.input.username != "" && this.input.password != "") {
                   axios.get("http://127.0.0.1:8000/loginweb?username="+this.input.username+"&password="+this.input.password)
                   .then((response) => {
                        var loggedIn =  response.data[0].Success
                            if(loggedIn)
                            {
                                this.vals={}
                                console.log(loggedIn)
                                loggedIn.forEach(element => {
                                 if(this.vals[element[0]]===undefined)
                                 {
                                    this.vals[element[0]]=[element] 
                                 }  
                                 else
                                 {
                                  this.vals[element[0]].push(element) ;  
                                 }
                                });

                                this.set=true
                                this.$emit("authenticated", true);
                            }
                   })
                   .catch((error) => {
                            console.log(error);
                   })
                   
                    
                } else {
                    console.log("A username and password must be present");
                }
            }
        }
    }
</script>

<style>

#login .form-inputs {
    padding-bottom: 10px;
}

#login .form-inputs label {
    padding-right: 10px;
}

</style>