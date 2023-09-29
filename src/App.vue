<template>
  <div class="pagina">
    <header class="header1">
      <div class="filtro-tipo">
        <select id="tipo" class="tpo" v-model="tipoSeleccionado">
          <option value="">Todos</option>
          <option  v-for="tipo in tiposPokemon" :key="tipo" :value="tipo"  :style="{ backgroundColor: getColorByType(tipo) }"  >{{ tipo }}</option>
        </select>
      </div>

    </header>
    <div class="contenedor1">
      <div class="contenedor-imgp">
        <div class="input-container">
          <input type="text" placeholder="Buscar Pokemon" v-model="busqueda" />
          <button class="button1"></button>
        </div>
      </div>
    </div>

    <!-- la parte de las tarjetas -->
    <h1 class="titulo1"><img src="https://fontmeme.com/permalink/230923/bfc1659f192f507f20d726210037de5a.png"
        alt="fuente-pokemon" border="0"></h1>
    <div class="contenedor2">
      <div class="tarjet" v-for="(pokemon, index) in pokemonesFiltrados" :key="index" @click="mostrarModal(pokemon)">
        <div class="nombre-conter">
          <p class="infor3" :style="{ backgroundColor: getColorByType(pokemon.tipo_pk[0]) }">{{ pokemon.nombre }}</p>
          <h5 class="edg">EDG</h5>
          <p class="infor2" :style="{ backgroundColor: getColorByType(pokemon.tipo_pk[0]) }">#{{ pokemon.numero }}</p>
        </div>
        <div class="img-tarjet">
          <img class="imagen-pokemon" :src="pokemon.img" alt="" />
        </div>
        <div class="info2">
          <p>{{ pokemon.descripcion }}</p>
          <div class="tipo-logo">
            <div class="tipo1-1">
              <p class="tipo1-color" v-for="(tipo, index) in pokemon.tipo_pk" :key="index"
                :style="{ backgroundColor: getColorByType(tipo) }">{{ tipo }}</p>
            </div>
            <div class="tipo-cuadro"></div>
          </div>
        </div>
        <div class="color-bajo" :style="{ backgroundColor: getColorByType(pokemon.tipo_pk[0]) }"></div>
      </div>
    </div>
    <button class="btn-mas" @click="obtenerUrlpokemon">Mas +</button>



    <!-- Modal -->
    <div v-if="info_modal" class="modal">
      <div class="modal-content">
        <button @click="ocultarModal" class="btn2">❌</button>
        <div class="container1">
          <header class="header2" :style="{ backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }"></header>
          <div class="container overflow-hidden text-center">
            <div class="row gx-0">
              <div class="col">
                <div class="p-5">
                  <div class="pg-2">
                    <div class="primera-p">
                      <label class="titulo1-1" for="">ID</label>
                      <p class="infoc" :style="{ color: getColorByType(selectedPokemon.tipo_pk[0]) }">#{{
                        selectedPokemon.numero }}</p>
                    </div>

                    <div class="primera-p">
                      <label class="titulo1-1" for="">Height</label>
                      <p class="info">{{ selectedPokemon.altura }}cm</p>
                    </div>

                    <div class="primera-p">
                      <label class="titulo1-1" for="">Weight</label>
                      <p class="info">{{ selectedPokemon.peso }}kg</p>
                    </div>

                    <div class="primera-p">
                      <label class="titulo1-1" for="">Type</label>
                      <p class="tipo2-color" v-for="(tipo, index) in selectedPokemon.tipo_pk" :key="index"
                        :style="{ backgroundColor: getColorByType(tipo) }">{{ tipo }}</p>
                    </div>
                  </div>
                </div>
              </div>

              <div class="col">
                <div class="p-5" id="txt">
                  <h1 class="titulo2" :style="{ backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">{{
                    selectedPokemon.nombre }}</h1>
                  <img :src="selectedPokemon.img" alt="" class="imagenPK" />
                </div>
              </div>

              <div class="col">
                <div class="p-5">
                  <div class="pg-3">
                    <div class="contt">
                      <label for="HP">HP</label>
                      <div class="progress" role="progressbar" aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.hp" aria-valuemin="0" aria-valuemax="100">
                        <div class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.hp + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">
                          {{ selectedPokemon.hp }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Attack">Attack</label>
                      <div class="progress" role="progressbar" aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.ataque" aria-valuemin="0" aria-valuemax="100">
                        <div class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.ataque + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">
                          {{ selectedPokemon.ataque }}%</div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Defence">Defence</label>
                      <div class="progress" role="progressbar" aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.defensa" aria-valuemin="0" aria-valuemax="100">
                        <div class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.defensa + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">
                          {{ selectedPokemon.defensa }}%</div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Sp. Attack">Sp. Attack</label>
                      <div class="progress" role="progressbar" aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.ataque_especial" aria-valuemin="0" aria-valuemax="100">
                        <div class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.ataque_especial + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">
                          {{ selectedPokemon.ataque_especial }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Sp. Defence">Sp. Defence</label>
                      <div class="progress" role="progressbar" aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.defensa_especial" aria-valuemin="0" aria-valuemax="100">
                        <div class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.defensa_especial + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">
                          {{ selectedPokemon.defensa_especial }}%
                        </div>
                      </div>
                    </div>

                    <div class="contt">
                      <label for="Speed">Speed</label>
                      <div class="progress" role="progressbar" aria-label="Animated striped example"
                        :aria-valuenow="selectedPokemon.velocidad" aria-valuemin="0" aria-valuemax="100">
                        <div class="progress-bar progress-bar-striped progress-bar-animated"
                          :style="{ width: selectedPokemon.velocidad + '%', backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">
                          {{ selectedPokemon.velocidad }}%</div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="barra-baja" :style="{ backgroundColor: getColorByType(selectedPokemon.tipo_pk[0]) }">EDWIN(EDG)</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, computed } from "vue";

let pokemones = ref([]);
let info_modal = ref(false);
let selectedPokemon = ref(null);
let offset = ref(0);
let tipoSeleccionado = ref(null);
let busqueda = ref("");

const pokemonesFiltrados = computed(() => {
  const terminoBusqueda = busqueda.value.toLowerCase();
  const tipoFiltrado = tipoSeleccionado.value;

  return pokemones.value.filter((pokemon) => {
    const nombreCoincide = pokemon.nombre.toLowerCase().includes(terminoBusqueda);
    const tipoCoincide = !tipoFiltrado || pokemon.tipo_pk.includes(tipoFiltrado);

    return nombreCoincide && tipoCoincide;
  });
});

async function obtenerUrlpokemon() {
  const limit = 50;
  const url = `https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${offset.value}`;

  let response = await axios.get(url);
  let pokemonData = response.data.results;

  for (let item of pokemonData) {
    let r = await axios.get(item.url);
    let pokemon = {
      img: r.data.sprites.other["official-artwork"].front_default,
      numero: r.data.id,
      nombre: r.data.name,
      altura: r.data.height,
      peso: r.data.weight,
      tipo_pk: r.data.types.map((pk) => pk.type.name),
      hp: r.data.stats[0].base_stat,
      ataque: r.data.stats[1].base_stat,
      defensa: r.data.stats[2].base_stat,
      ataque_especial: r.data.stats[3].base_stat,
      defensa_especial: r.data.stats[4].base_stat,
      velocidad: r.data.stats[5].base_stat,
      descripcion: ""
    };

    let especieResponse = await axios.get(`https://pokeapi.co/api/v2/pokemon-species/${r.data.id}/`);
    let descripcion = especieResponse.data.flavor_text_entries.find(entry => entry.language.name === "es").flavor_text;

    pokemon.descripcion = descripcion;
    pokemones.value.push(pokemon);
  }

  offset.value += limit;
}

function mostrarModal(pokemon) {
  selectedPokemon.value = pokemon;
  info_modal.value = true;
}

function ocultarModal() {
  info_modal.value = false;
}


function getColorByType(tipo) {
  if (tipo === "fire") {
    return "#ff8a80";
  } else if (tipo === "grass") {
    return "#67e696";
  } else if (tipo === "electric") {
    return "#f7f72a";
  } else if (tipo === "water") {
    return "#81d4fa";
  } else if (tipo === "ground") {
    return "#f4e7da";
  } else if (tipo === "rock") {
    return "#544a28";
  } else if (tipo === "fairy") {
    return "#f8bbd0";
  } else if (tipo === "poison") {
    return "#ad8ee7";
  } else if (tipo === "bug") {
    return "#b5f57d";
  } else if (tipo === "dragon") {
    return "#37cc73";
  } else if (tipo === "psychic") {
    return "#eaeda1";
  } else if (tipo === "flying") {
    return "#c0daea";
  } else if (tipo === "fighting") {
    return "#de664e";
  } else if (tipo === "normal") {
    return "#bcaaa4";
  } else if (tipo === "dark") {
    return "#1c121f";
  } else {
    return "white";
  }
}

const tiposPokemon = [
  "fire",
  "grass",
  "electric",
  "water",
  "ground",
  "rock",
  "fairy",
  "poison",
  "bug",
  "dragon",
  "psychic",
  "flying",
  "fighting",
  "normal",
  "dark"
];

obtenerUrlpokemon();
</script>

<style scoped>
.contenedor1 {
  height: 60vh;
}

.contenedor-imgp {
  background-image: url('assets/pk2.PNG');
  background-size: cover;
  background-position: center;
  width: 100%;
  height: 100%;
}

.header1 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 56px;
  display: flex;
  align-items: center;
  background-color: #79787859;
}

.filtro-tipo {
  margin-left: 35px;
  height: 40px;
  width: 40px;
  align-items: center;
  display: flex;
  justify-content: center;
}

.contenedor2 {
  display: grid;
  background-image: url('assets/pk3.PNG');
  background-repeat: no-repeat;
  background-position: center;
  grid-template-columns: repeat(4, 1fr);
  margin: 10px;
  gap: 20px;
  font-family: 'Indie Flower', cursive;
}


.tipo1-color {
  margin: 5px;
  font-weight: 700;
  text-align: center;
  width: 100%;
  border: 1px solid black;
  border-radius: 15px;
}

.tipo2-color {
  margin-left: 20px;
  font-weight: 500;
  text-align: center;
  width: 55px;
  border: 1px solid black;
  border-radius: 15px;
}

.tarjet {
  cursor: pointer;
  width: 100%;
  height: 450px;
  border-radius: 15px;
  border: 10px solid rgba(177, 177, 177, 0.692);
  margin: 1px;
}

.pepe {
  border-radius: 50%;
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 25px;
}

.pepe:hover {
  cursor: pointer;
  background-color: #75747465;
}

i {
  color: #ffffff;
}


.img-tarjet {
  text-align: center;
  border: 1px solid rgb(112, 112, 112);
  height: 50%;
  background-image: url('https://assets.pokemon.com//assets/cms2-es-es/img/misc/virtual-backgrounds/masters/forest.jpg');
  background-size: cover;
  background-position: center;
}

.titulo1 {
  text-align: center;
}

.input-container {
  width: 35%;
  position: absolute;
  left: 50%;
  top: 45%;
  transform: translate(-50%, -50%);
}

.imagen-pokemon {
  position: relative;
  top: 35px;
  width: 150px;
  height: 120px;
}

.modal {
  position: fixed;
  display: flex;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(43, 42, 42, 0.623);
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal-content {
  max-width: 95%;
  width: auto;
  max-height: 95vh;
  overflow-y: auto;
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgb(254, 255, 252);
}

.modal-content {
  background-image: url('https://i.postimg.cc/nLwDRS7k/pkw.png');
  background-size: cover;
  background-position: center;
}

.btn2 {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: transparent;
  border: 0;
  cursor: pointer;
}

.btn-mas {
  margin-left: 94%;
  width: 5%;
  margin-bottom: 10px;
  border-radius: 20px;
}


.btn-mas:hover {
  background: linear-gradient(50deg, rgb(43, 42, 42), rgb(255, 255, 255), rgb(47, 48, 47));
}

/* informacion de pokemon */
.header2 {
  height: 60px;
  border-radius: 10px 10px 0px 0px;
  background-color: rgb(84, 85, 85);
}

.titulo2 {
  position: relative;
  left: 50px;
  color: #ffffff;
  border-radius: 50px;
  bottom: 40px;
}

.pg-2 {
  position: relative;
  top: 25px;
  perspective: 300px;
  transition: .3s;
  transform: perspective(300px) rotateY(30deg);
}

.pg-2:hover {
  transform: perspective(300px) rotateY(0deg);
}

.pg-3:hover {
  transform: perspective(300px) rotateY(0deg);
}

.pg-3 {
  position: relative;
  top: 25px;
  width: 130%;
  perspective: 300px;
  transition: .3s;
  transform: perspective(300px) rotateY(-30deg);
}

.primera-p {
  display: flex;
  justify-content: space-between;
  margin: 25px;
}

.primera-p {
  display: flex;
  justify-content: space-between;
  margin: 25px;
}

label {
  font-weight: 700;
  color: #222121;
  font-size: 15px;
}

.info2 {
  background-image: url('assets/Captura.PNG');
  background-size: cover;
  background-position: center;
  height: 42%;
}

.imagenPK {
  position: relative;
  width: 200%;
  right: 80px;
  bottom: 50px;
  height: 340px;
}

.infoc {
  font-size: 30px;
  font-weight: 800;
}

.barra-baja {
  color: transparent;
}

.contt {
  display: flex;
  justify-content: flex-end;
  width: 400px;
  align-items: center;
}

.progress-bar {
  text-align: right;
  font-weight: 500;
  font-size: 18px;
  color: rgb(29, 26, 26);
  border-radius: 5px;
  transition: background-color 0.4s ease;
}

.progress {
  background-color: rgb(228, 224, 224);
  border: 1px solid rgb(56, 56, 56);
  height: 20px;
  margin: 15px;
  margin-left: 25px;
  width: 200px;
}

@media screen and (max-width: 1200px) {

  .pg-2,
  .pg-3 {
    transform: none;
  }

  .titulo2 {
    width: 70%;
  }

  .input-container {
    position: relative;
    bottom: 20px;
  }

  .contenedor1 {
    height: 40vh;
  }

  .contenedor-imgp {
    background-color: #222121;
    background-image: url('assets/pk2.PNG');
    background-size: cover;
    background-position: center;
  }

  .contenedor2 {
    grid-template-columns: repeat(2, 1fr);
  }

  .pg-3 {
    margin-top: 0;
    position: relative;
    right: 105px;
  }

  .imagenPK {
    position: relative;
    width: 300px;
    height: 350px;
  }
}

@media screen and (max-width: 380px) {
  .modal-content {
    max-width: 100%;
  }

  .contt {
    flex-direction: column;
    position: relative;
    top: -90px;
  }

  .img-tarjet {
    width: 100%;
  }
}

@media (max-width: 500px) {
  .contenedor2 {
    grid-template-columns: repeat(1, 1fr);
    /* Cambiamos a 2 columnas para pantallas más pequeñas */
  }

  .contenedor1 {
    height: 20vh;
  }

  .contenedor-imgp {
    background-color: #222121;
    background-image: url('assets/pk2.PNG');
    background-size: cover;
    background-position: center;
  }


}
</style>
