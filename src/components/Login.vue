<template>
    <div class="row">
        
        <div class="flex col s12 m8 l8 xl6 offset-m2 offset-l2 offset-xl3 card-panel">
            <div class="col s12">
                <div class="row">
                    <!-- blue lighten-3 -->
                    <div class="title col s12 indigo indigo accent-2 valign-wraper">
                        <h4>Access</h4>
                    </div>
                </div>
            </div>
            <form @submit.prevent="localLogin">
                <div class="row">
                    <div class="col s12 m8 offset-m2">
                        <label>User</label>
                        <input type="text" v-model="user">
                    </div>
                </div>
                <div class="row">
                    <div class="col s12 m8 offset-m2">
                        <label>Password</label>
                        <input type="password" v-model="password">
                    </div>
                </div>
                
                <div class="row">
                    <div class="col s12 center-align">
                        <a href="#!"><p class="blue-text text-darken-1">¿Forgot your password?</p></a>
                        <a href="#!" @click="openModal"><p class="bolder blue-text text-darken-1">Create account</p></a>
                    </div>
                </div>

                <div class="row">
                    <div class="col s12 center-align">
                        <button v-show="!loading" type="submit" class="btn indigo accent-2">Login</button>
                        <div  v-show="loading" class="progress indigo accent-2">
                            <div class="determinate white"></div>
                        </div>
                    </div>
                </div>
                    <!-- <div  v-show="loading" class="progress indigo accent-2">
                        <div class="determinate white"></div>
                    </div> -->
            </form>
        </div>

    </div>

    <!-- Modal register -->
    <div class="modal" id="register-modal">
        <div class="modal-content">
            <div class="row">

                <!-- Head -->
                <div class="col s12">
                    <div class="row">
                        <div class="col s12 center-align">
                            <img src="../assets/Slogan/academy.png" alt="Slogan academic">
                        </div>
                    </div>
                    <div class="row">
                        <div class="title col s12 indigo blue lighten-3 valign-wraper">
                            <h4>Register</h4>
                        </div>
                    </div>
                </div>

                <!-- Inputs -->
                <div class="row">
                    <div class="col s12 m12">
                        <input type="text" placeholder="Name" v-model="rName">
                        <span class="helper-text"></span>
                    </div>
                    <div class="col s12 m6">
                        <input type="text" placeholder="Email" v-model="rEmail">
                        <span class="helper-text"></span>
                    </div>
                    <div class="col s12 m6">
                        <input type="text" placeholder="Phone" v-model="rPhone">
                        <span class="helper-text"></span>
                    </div>
                    <div class="col s12 m6">
                        <div class="input-field" id="select-rol">
                            <select v-model="rTipe_document">
                                <option value="" disabled selected>Choose your document type</option>
                                <!-- Cedula de Ciudadania -->
                                <option value="6341b482ee0c46a68e80febc">Citizenship card</option>
                                <!-- Tarjeta de Identidad -->
                                <option value="6341b494ee0c46a68e80febd">Identity card</option>
                                <!-- Pasaporte -->
                                <option value="6341b4cfee0c46a68e80febe">Passport</option>
                                <!-- Cedula Extrangera -->
                                <option value="6341b4e0ee0c46a68e80febf">Foreign ID</option>
                            </select>
                        </div>
                    </div>
                    <div class="height_adjustment col s12 m6">
                        <input type="text" placeholder="Document number" v-model="rdocument">
                        <span class="helper-text"></span>
                    </div>
                    <div class="col s12 m4">
                        <div class="input-field" id="select-rol">
                            <select>
                                <option value="" disabled selected>Choose your rol</option>
                                <option value="6341b4f9ee0c46a68e80fec0">Profesor</option>
                                <option value="6341b509ee0c46a68e80fec1">Alumno</option>
                            </select>
                        </div>
                    </div>
                    <div class="height_adjustment col s12 m4">
                        <input type="text" placeholder="Username" v-model="rUsername">
                        <span class="helper-text"></span>
                    </div>
                    <div class="height_adjustment col s12 m4">
                        <input type="password" placeholder="PassWord" v-model="rPassWord">
                        <span class="helper-text"></span>
                    </div>
                </div>

                <!-- Send -->
                <div class="row center-align">
                    <p>
                        <label>
                            <input type="checkbox" class="filled-in indigo blue lighten-3"/>
                            <span>I accept the Terms of Service</span>
                        </label>
                    </p>
                    <button class="btn indigo blue lighten-3">
                        <div class="valign-wrapper">
                            <span class="mr">Send</span>
                            <i class="material-icons">send</i>
                        </div>
                    </button>
                </div>

            </div>
        </div>
    </div>
</template>


<script>
// import axios from 'axios';
import M from 'materialize-css'

export default {
    name: 'app-login',
    data() {
        return {
            // Login
            mesage: "Login",
            user: '',
            password: '',
            loading: false,
            
            // Register
            rUsername: '',
            rPassWord: '',
            rName: '',
            rTipe_document: '',
            rdocument: '',
            rEmail: '',
            rPhone: '',
            rRol: '',

            modales: [],
        }
    },
    mounted() {
        var elems = document.querySelectorAll('.modal');
        this.modales = M.Modal.init(elems, null);

        var select = document.querySelectorAll('select');
        M.FormSelect.init(select);
    },
    methods: {
        // async login() 
        // {
        //     var payload = {
        //         userinfo: this.user,
        //         passWord: this.password
        //     };
        //     this.loading = true;
        //     // console.log(JSON.stringify(payload))
            
        //     // await axios.post('http://54.242.120.40:3000/auth/login', {"userinfo": "Juan", "passWord": "contrasena"}, { headers: {'Content-Type': 'application/json'}})
        //     //     .then(response => {
        //     //         console.log({response})
        //     //         // this.axios.defaults.headers.common['Authorization'] = 'Bearer ' + response.data.token;
        //     //         // localStorage.setItem('token', response.data.token);
        //     //         // this.$store.commit('setAuthenticated', true);
        //     //         // console.log(response.data.userData.userName);
        //     //         // this.$store.commit('setUsername', response.data.userData.userName);
        //     //         // this.$store.commit('setId', response.data.userData.id);
        //     //         // this.$router.push('/');
        //     //     })
        //     //     .catch(error => {
        //     //         var data = error.response.data;
        //     //         M.toast({html: data.message});
        //     //     });
        //     axios({
        //         // method: 'post',
        //         method: 'get',
        //         // url: 'http://54.242.120.40:3000/auth/login',
        //         url: 'http://54.242.120.40:3000/user/all',
        //         // data: {
        //         //     "userinfo": "Juan",
        //         //     "passWord": "contrasena"
        //         // },
        //         headers: {
        //             'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MzRhMTk4YzRkYzBiMDU4NzYxZDQyMzUiLCJuYW1lIjoiSnVhbiBDYW1pbG8gUGVyZXogUGVyZXoiLCJ1c2VybmFtZSI6Ikp1YW4iLCJlbWFpbCI6Imp1YW4ucGVyZXpAZ21haWwuY29tIiwicm9sIjoiNjM0MWI0ZjllZTBjNDZhNjhlODBmZWMwIiwiaWF0IjoxNjY1ODYwNDg0LCJleHAiOjE2NjU5MDM2ODR9.11nYtN6qZpLBRVPk47DiUN0Yel2DK5904ezDUNfw2Ys',

        //         }
        //     })
        //     .catch(error => {
        //             var data = error.response.data;
        //             // M.toast({html: data.message});
        //             console.log("ERROR", data.message);
        //         });

        //     this.loading = false;
        // },
        async localLogin() {
            this.loading = true;
            
            this.axios.defaults.headers.common['Authorization'] = 'Bearer ' + 'TokenDePrueba';
            localStorage.setItem('token', 'TokenDePrueba');
            this.$store.commit('setAuthenticated', true);
            this.$store.commit('setUsername', this.user);
            this.$store.commit('setId', 'id123');
            this.$router.push('/');

            this.loading = false;
        },
        openModal () {
            var modal_register = M.Modal.getInstance(document.querySelector('#register-modal'));
            modal_register.open();
        },
        
        // async getUser() {
        //     var config = {
        //         method: 'get',
        //         url: 'http://54.242.120.40:3000/user/all',
        //         headers: { 
        //             'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MzRhMTk4YzRkYzBiMDU4NzYxZDQyMzUiLCJuYW1lIjoiSnVhbiBDYW1pbG8gUGVyZXogUGVyZXoiLCJ1c2VybmFtZSI6Ikp1YW4iLCJlbWFpbCI6Imp1YW4ucGVyZXpAZ21haWwuY29tIiwicm9sIjoiNjM0MWI0ZjllZTBjNDZhNjhlODBmZWMwIiwiaWF0IjoxNjY1ODYwNDg0LCJleHAiOjE2NjU5MDM2ODR9.11nYtN6qZpLBRVPk47DiUN0Yel2DK5904ezDUNfw2Ys'
        //         }
        //     };
        //     axios(config)
        //     .then(function (response) {
        //         console.log(JSON.stringify(response.data));
        //     })
        //     .catch(function (error) {
        //         console.log(error);
        //     });
        // }

        // async registerUser() {
        //     let data = {
        //         username: "Pedro",
        //         passWord: "contrasena",
        //         name: "Pedro Camilo Mendoza Perez",
        //         tipe_document: "6341b482ee0c46a68e80febc",
        //         document: 1040408561,
        //         email: "pedro@gmail.com",
        //         phone: 3112365251,
        //         rol: "6341b4f9ee0c46a68e80fec0"
        //     }
        // }
        
        
    }
}
</script>


<style scoped>
.title {
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.title h4 {
    margin: 0;
    color: #ffffffe6;
}

.bolder {
    font-weight: bolder;
}

.height_adjustment {
    padding-top: 15px;
    margin-bottom: 7px;
}
.mr {
    margin-right: 8px;
}
</style>
  