<template>
    <link href="https://fonts.googleapis.com/css2?family=Josefin+Sans:wght@500&display=swap" rel="stylesheet">

    <div id='project-pokedex'>
        <div class='container-title'>
            <h1 class="title"> Pokedex </h1>
            <p  class="subtitle"> 
                Made with <strong>Vue</strong>.js and the <a href="">PokeApi.co</a> data! 
            </p>
        </div>
        
        
        <Pokedex /> 
        
        
    </div>
</template>

<script>
import Pokedex from './components/pokedex'

export default {
    data(){
        return {

        }
    },
    components: { Pokedex }

}
</script>

<style>    
    html, body{
        min-width: 350px;
    }

    

    .container-title{
        margin-top: 5%;
    }

    .title{
        text-align: center;
        font-size: 45px;
        font-family: 'Josefin Sans', sans-serif;
    }

    .title::after{
        width: 200px;
        height: 3px;
        background: black;
        display: block;
        content: '';
        margin: auto;
        margin-bottom: 1%;
    }

    .subtitle{
        text-align: center;
        font-family: 'Josefin Sans', sans-serif;
    }

    .subtitle strong{
        color: #41b883;
    }

    @media (min-width: 300px) and (max-width: 450px){
        .subtitle{
            margin-top: 2%;
        }
    }

    .btn-gen{
        width: 80px;
        height: 40px;
    }

    
</style>