<template lang="html">
    <div>
        <h3>
            Hello this is my first project VueJs
        </h3>
        <ul>
            <li v-for="character in characters">
                {{character.name}}
            </li>
        </ul>
    </div>

</template>

<script>
import {secret_key, public_key} from '../marvel.js'
import axios from 'axios'
import Crypto from 'crypto-js'
export default {
    name: 'Characters',
    data() {
        return {
            characters: []
        }
    },
    mounted() {
        console.log('call get Characters')
        this.getCharacters();
    },
    methods: {
        getCharacters: function() {
            axios.get(`http://gateway.marvel.com/v1/public/characters?ts=1&apikey=${public_key}&hash=${Crypto.MD5('1'+secret_key+public_key).toString()}`)
            .then( (result) => {
                
                result.data.data.results.forEach(element => {
                    console.log(JSON.stringify(element, null, 2))
                    this.characters.push(element)
                });
            })
            .catch((error) => {
                console.log('quan; ' +JSON.stringify(error, null, 2))
            });
        },
        hashCode: function(s){
            return s.split("").reduce(function(a,b){a=((a<<5)-a)+b.charCodeAt(0);return a&a},0);              
        }

    }
}
</script>

<style lang="css">

</style>


