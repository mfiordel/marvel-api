<template lang="html">
    <div class="container">
        <div class="card">
            <h3>Character View</h3> 
            <ul>
                <h4>Personaje Id</h4>
                <p>{{ this.$route.params.id}}</p>
                <li v-for="char in character">
                    <h4>Nombre del Personaje</h4>
                    <h5>{{char.name}}</h5>
                    <h4>Descripcion</h4>
                    <p>{{char.description}}<span v-if="char.description === ''">Sin Descripcion</span></p>
                    <h4>Series</h4>
                    <p>{{char.series.available}}</p>
                    
                </li>
            </ul>
            <img :src="url" alt="">
        </div>
    </div>
</template>
<script>
    import {public_key} from '../marvel'
    import axios from 'axios'
    export default {
        name: 'CharacterView',

        data(){
            return{
                character: [],
                url: '',
                size: 'standard_large.jpg'

            }
        },

        mounted(){
            this.getCharacter()  
        },
        methods: {
            getCharacter: function(){
                var characterId = this.$route.params.id
                axios.get(`http://gateway.marvel.com/v1/public/characters/${characterId}?apikey=${public_key}`)
                .then((res)=>{
                   
                    res.data.data.results.forEach((e)=>{
                        this.character.push(e)

                        this.url= `${e.thumbnail.path}/${this.size}`
                    })
                })
                .catch((err)=>{
                    console.log(err)
                })
            }            
        },
    }
</script>
<style lang="css">
body {
        margin: 0;
        padding: 0;
    }
    .container {
        background-color: rgb(255, 255, 255);
        border: solid rgb(61, 5, 5);
        border-width: 2px;
        margin: 5%;
        display: flex;
        justify-content: center;
       
    }
    .card {
        text-align: center;
        border-style:groove;
        border-width: 2px;
        border-radius: 5%;
        box-shadow: 5px 5px 20px rgb(43, 41, 53);
    }

    h4{
        font-variant: bold;
        font-size: x-large;
    }

    li{
        list-style: none;
    }
</style>