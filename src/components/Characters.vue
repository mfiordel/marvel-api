<template lang="HTML">
    <div class="container">
            <h3>Marvel API with Vue.js</h3>
        <ul class="card-container">
            <li v-for="character in characters" class="card">
                <router-link :to="{ name: 'character', params: { id: character.id } }"><h4> {{character.name}}</h4> </router-link>   
                <p>Id: {{character.id}}</p>
                <p>Series disponibles: {{character.series.available}}</p>
                <button class="btn-primary" @click="deleteChar(index)">Eliminar</button>

            </li>
        </ul>       
    </div>
    
</template>
<script>
import { public_key, secret_key } from '../marvel'
import axios from 'axios'
export default{
    name: 'Characters',

    data(){
        return {
            characters: []
        }
    },
    mounted(){
        this.getCharacters()
    },

    methods:{
    getCharacters: function(){
      axios.get(`http://gateway.marvel.com/v1/public/characters?apikey=${public_key}`)
      .then((res) => {
       
        res.data.data.results.forEach((e)=>{
            this.characters.push(e)
        })

      })
      .catch((err) => {
        console.log(err)
      })
    },
    deleteChar: function(index) {
        this.characters.splice(index, 1);
        localStorage.setItem('char-vue', JSON.stringify(this.characters))
        
    }
  },
  created: function(){
        let dataDB = JSON.parse(localStorage.getItem('char-vue'));
        if (dataDB === null){
            this.characters= [];
        }else{
            this.characters = dataDB;
        };
    }   
    
}
</script>

<style lang="css">
    body {
        margin: 0;
        padding: 0;
    }
    .container {
        background-color: rgb(255, 255, 255);
        border: none;
        box-shadow: 5px 5px 20px black;
        margin: 5%;
       
    }
    .card-container{
        display: grid;
        grid-template-columns: auto auto auto;
        grid-gap: 30px;
        margin: 20px 20px 20px 20px;
        justify-content: space-between;
        
    }
    .card {
        width: 60%;
        height: 60%;
        text-align: center;
        border: none;
        padding: 50px;
        border-radius: 20%;
        box-shadow: 5px 5px 20px black;
    }

    h3{
        font-variant: bold;
        font-size: x-large;
        text-align: center;
    }

    li{
        list-style: none;
    }
</style> 