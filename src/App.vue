<template>
  <div>
    <nav>
      <img  src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1200px-International_Pok%C3%A9mon_logo.svg.png" class="img_logo" />
      <h1>Escribe el Nombre del Pokemon a Buscar</h1>
      <div class="poke_input">
        <!-- input v-model para realizar two way data bindng -->
        <input v-model="nombre" type="text" @keyup.enter="buscar_pokemones" />
        <!-- evento click con funcion buscar pokemones en created. -->
        <button @click="buscar_pokemones">Buscar</button>
      </div>
    </nav>
    <div class="cuerpo_poke">
      <!-- resultados de pokemones -->
      <div class="info_pokemon">
        <!-- nombre pokemon mayusculas! -->

         <h2 class="pokeNombre">{{ nombre_pokemon.toUpperCase() }}</h2>
      <!-- v-bind para acortar imagen -->
        <img :src="imagen" alt />
      
        <!-- enlazando datos con uso de interpolacion (doble moustache) -->
     
        <h6>
          <strong>Movimiento POKEMON</strong>
        </h6>
        <ul>
          <!-- recorremos el arreglo con directiva v-for 
          para vincular atributos html en el modelo, usamos :key para 
          mantener el orden en caso de errores
          mapeando movimientos matriz de origen - movimiento es un "alias" para
          el elemnto que se esta iterando
          -->
          <li v-for="(movimiento, i) in movimientos" :key="i">{{ movimiento.move.name }}</li>
        </ul>
        <h6>
          <strong>Habilidades</strong>
        </h6>
        <ul>
          <!-- repetimos accion de directiva v-for anterior--> 
          <li v-for="(habilidad,i) in habilidades" :key="i">{{ habilidad.ability.name }}</li>
        </ul>
              
       
        
      
      </div>
    </div>


  </div>
</template>

<script>
export default {
  name: 'App',
  //asigno objetos en mi data
  data () {
    return {
      //variable necesaria para el input
      //en el estado no se crean LET, debe ser --PROPIEDAD Y VALOR--
      nombre : 'pikachu',
      //en variable pokemon, obtendremos datos que rescatamos de la api
      pokemon : {
        name : '',
        sprites : { front_default:""},
        moves: [],
        abilities: [],
      },
    }

  },
  //metodo que procesa logica -- antes que los datos se muestren
  created() {
    this.buscar_pokemones();
  },
  methods : {
    //fetch para trabajar con valores de api "LLAMADO DE API"
    buscar_pokemones(){
      fetch(`https://pokeapi.co/api/v2/pokemon/${this.nombre}`)
      //esperando respuesta de api 
      .then(function(respuesta_api){

        //retorna  data  modo JSON
        return respuesta_api.json();
      })
      // se guardan datos en variable pokemon --> JSON
      .then((my_json) => {
      console.log(my_json);
      this.pokemon = my_json;
      });
    },
  },

  /* propiedad computada, dependiente de la propiedad pokemon y tomamos los
  valores que le dimos en data()
  COMPUTED PROPERTIES no es igual a un method comun. */
  computed : {
    imagen(){
      return this.pokemon.sprites.front_default;
    },

    nombre_pokemon(){
      return this.pokemon.name
    },
    //uso de slice para poner limite de busqueda
    movimientos(){
    return this.pokemon.moves.slice(0,10);
      },
      habilidades(){
        return this.pokemon.abilities.slice(0,5);
      },
  },
 
};
</script>

<style>
nav {
  width: 100%;
  height: auto;
  display: flex;
  justify-content: space-around;
  color: blue;
  position: relative;
  background:white
}
.img_logo {
  width: 200px;
  height: 100px;
 
}

.info_pokemon {
  border: 5px solid blue;
}


@media(max-width:600px) {
  .img_logo {
    display: none;
  }
  button {
  background-color: red;
  margin-top: 10px;
}
}
.poke_input {
 margin-top: 15px;}

.cuerpo_poke {
  align-content: center;
  display: flex;
  justify-content: center;
  color: rgb(121, 121, 207);
  font-family: serif;
  font-size: 20px;
}
h2 {
  color: black;
 
}
button {
  padding: 3px ;
  color: coral;
  background-color: white;
  margin-left: 5px;
}
input {
  padding: 5px;
}
</style>