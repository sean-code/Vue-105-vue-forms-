<template>
    <form @submit.prevent="handleSubmit">
        <label>Email</label>
        <input type="email" required v-model="email">
        <label>Password</label>
        <input type="password" required v-model="password">
        <div v-if="passwordError" class="error">
            {{ passwordError }}
        </div>

        <label>Role</label>
        <select v-model="role">
            <option>Web Developer</option>
            <option>Web Designer</option>
        </select>


        <label>Country</label>
        <input type="text">
        <select v-model="selected">
            <option v-for="country in countries" :value="country.value">
            {{ country.label }}
            </option>
        </select>

        <!-- Keyboard Events -->
        <label>Skills</label>
        <input type="text" v-model="tempSkill" @keyup="addSkill">
        <div v-for="skill in skills" :key="skill" class="pill">
            <span @click="deleteSkill(skill)">{{ skill }}</span>
        </div>


        <!-- Checkbox -->
        <div class="terms">
            <input type="checkbox" required v-model="terms">
            <label>Accept Terms and Conditions</label>
        </div>
        <div>
            <input type="checkbox" value="Shawn" v-model="names">
            <label>Shawn</label>
        </div>
        <div>
            <input type="checkbox" value="Dre" v-model="names">
            <label>Dre</label>
        </div>
        <div>
            <input type="checkbox" value='Marion' v-model="names">
            <label>Marion</label>
        </div>
        <div  class="submit">
            <button>Create an Account</button>
        </div>
    </form>

    <p>Email: {{ email }}</p>
    <p> Password: {{ password }}</p>
    <p>Role: {{ role }}</p>
    <p>Terms Accepted: {{ terms }}</p>
    <p>Names: {{ names }}</p>

</template>


<script>

const countries = require('i18n-iso-countries')
countries.registerLocale(require('i18n-iso-countries/langs/en.json'))


    export default{
        name: 'CountrySelection',
        computed: {
            countries () {
             const list = countries.getNames('en', { select: 'official' })
             return Object.keys(list).map((key) => ({ value: key, label: list[key] }))
            }
        },
        data(){
            return{
                selected: null,
                email: '',
                password: '',
                role: '',
                terms: false,
                names: [],
                tempSkill: '',
                skills: [],
                passwordError: ''
            }
        },
        methods:{
            addSkill(e){
                console.log(e)
                if (e.key === ',' && this.tempSkill){
                    if( !this.skills.includes(this.tempSkill)){
                        this.skills.push(this.tempSkill)
                    }
                    this.tempSkill = ''
                }
            },
                deleteSkill(skill){
                    // console.log(skill)
                    this.skills = this.skills.filter((item) => {
                    return skill !== item
                })
            },
            handleSubmit(){
                // console.log("form Submitted")
                // Password validation
                this.passwordError = this.password.length > 5 ? '' : 'Password must be at least 6 characters'
            }
        }
    }
</script>

<style>
    form{
        max-width: 420px;
        margin: 30px auto;
        background: white;
        text-align: left;
        padding: 40px;
        border-radius: 10px;
    }
    label{
        color: #aaa;
        display: inline-block;
        margin: 25px 0 15px;
        font-size: 0.6em;
        text-transform: uppercase;
        letter-spacing: 1px;
        font-weight: bold;
    }
    input, select{
        display: block;
        padding: 10px 6px;
        width: 100%;
        box-sizing: border-box;
        border: none;
        border-bottom: 1px solid #ddd;
        color: #555;
    }

    input[type="checkbox"] {
        display: inline-block;
        margin: 0 10px 0 0;
        width: 16%;
        position: relative;
        top: 2px;
    }

    .pill{
        display: inline-block;
        margin: 20px 10px 0 0;
        padding: 6px 12px;
        background: #eee;
        border-radius: 20px;
        font-size: 12px;
        letter-spacing: 1px;
        color: #777;
        font-weight: bold;
        cursor: pointer;
    }

    button{
        background: #0b6dff;
        border: 0;
        padding: 10px 20px;
        margin-top: 20px;
        color: white;
        border-radius: 20px;
    }

    .submit{
        text-align: center;
    }
    .error{
        color: #ff0062;
        margin-top: 10px;
        font-size: 0.8em;
        font-weight: bold;
    }

</style>