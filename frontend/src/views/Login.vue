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
        <br/>
        <p>attempts per activity</p>
        <vue-bar-graph
  :points="attempts"
  :width="400"
  :height="200"
  :show-y-axis="false"
:show-x-axis="true"
  :use-custom-labels="true"
  :show-values="true"

/>

        <p>average time per activity</p>
        <vue-bar-graph
  :points="averageTimes"
  :width="400"
  :height="200"
  :show-y-axis="false"
:show-x-axis="true"
  :use-custom-labels="true"
  :show-values="true"

/>
        
        </div>
            


    </div>
</template>

<script>
    import axios from 'axios'
    import VueBarGraph from 'vue-bar-graph';

    export default {
        name: 'Login',
        components: {
    VueBarGraph,
},
        data() {
            return {
                input: {
                    username: "",
                    password: ""
                },
                set:false,
                vals:{},
                names:[],
                attempts:[],
                averageTimes:[]

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
                                for (let key in this.vals) {
                                    this.names.push(key)
                                    var attempts=0;
                                    var time=0;
                                    this.vals[key].forEach(element =>{
                                        attempts++;
                                        time+=element[3]
                                    })
                                    this.attempts.push({label: key, value: attempts});
                                    this.averageTimes.push({label: key, value: time/attempts})
  
                                }
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