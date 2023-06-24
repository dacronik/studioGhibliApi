<template>
    <div class="container">
        <button class="btn btn-success" @click="obtenerNombres()">Ingresar Datos</button>
        <section v-if="nombre">
            <div class="card">
                <div class="card-body">
                    <h2 class="card-title">Bienvenido @ {{fullName}}</h2>
                    <p class="card-text">Aqui podras encontar información de películas de anime de Studio Ghibli</p>
                    <btn class="btn btn-warning" @click="nombre=''">Limpiar información</btn>
                </div>
            </div>
            <div class="listado bg-info">
                <p>A continuación hay un listado con los nombres de las películas</p>
            </div>
            <div class="select">
                <select name="" class="form-select" v-model="filmSelected" @change="showInfo()">
                    <option v-for="movie in movies" :value="movie.name" :key="movie.name">{{movie.name}}</option>
                </select>
            </div>
            <div v-if="film">
                <div class="card text-black">
                    <div class="card-imagen">
                        <img class="card-img-top" :src="film.data.poster" alt="Title">
                    </div>
                    <div class="card-body">
                        <h4 class="card-title"><strong>Titulo Original: </strong>{{film.data.hepburn}}</h4>
                        <p class="card-text"><strong>Titulo en Inglés: </strong>{{film.name}}</p>
                    </div>
                    <div class="description">
                        <p><strong>Descripción: </strong>{{film.data.synopsis}}</p>
                    </div>
                    <div class="datos-extra">
                        <p><strong>Director: </strong>{{film.data.director}}</p>
                    </div>
                    <div class="datos-extra">
                        <p>Duración: <span>{{film.data.runtimeMinutes}} Minutos</span></p>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios';
export default {
    name: 'ghibli-api',
    // props: {},
    data: function(){
        return {
            nombre: '',
            apellido: '',
            filmSelected: '',
            movies:[],
            film:null,
        }
    },
    computed: {
        fullName(){
            return this.nombre + ' ' + this.apellido
        }
    },
    methods: {
        obtenerNombres(){
            let name = prompt("Ingrese su Nombre: ");
            let lastName = prompt("Ingrese su Apellido: ");
            this.nombre = name;
            this.apellido = lastName;
        },
        async consultaApi(){
            try{
                let response = await axios.get('https://studio-ghibli-films-api.herokuapp.com/api/');
                
                console.log(response);
                for(let key in response.data){
                    this.movies.push({
                        name:key,
                        data:response.data[key]
                    })
                }
                console.log(this.movies)
            }   
            catch(error){
                console.log(error)
            }
        },
        showInfo(){
            this.film = this.movies.find(movie => movie.name == this.filmSelected)
            console.log(this.film);
            console.log(this.filmSelected)
        }
    },
    // watch: {},
    // components: {},
    // mixins: [],
    // filters: {},
    // -- Lifecycle Methods
    created(){
        this.consultaApi()
    }
    // -- End Lifecycle Methods
}
</script>

<style scoped>
    .card{
        box-shadow: 5px 0 5px rgba(238, 18, 18, 0.5), -5px 0 5px rgba(238, 18, 18, 0.5), 0 5px 5px rgba(238, 18, 18, 0.5);
        margin: 20px 300px;
    }
    .listado{
        margin: 20px 200px;
        height: 100px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-left: 3px solid blue;
    }
    .card{
        display: flex;
        align-items: center;
        margin-top: 20px;
    }
    .card-imagen{
        width: 250px;
        margin-top: 20px;
    }
    .datos-extra{
        border: 1px solid #ccc;
        width: 90%;
        display: flex;
        align-content: center;
        justify-content: center;
        margin-bottom: 20px;
    }
    span{
        color: #ccc;
    }
</style>