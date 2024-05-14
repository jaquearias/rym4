<template>
  <div class="hello">
    <div class="row">
      <h1 class="titulo">Characters List</h1>
      <!-- Filtros -->
      <div class="col fondo"> 
        <h1 class="subtitulo">Filter</h1>
        <div class="filtro">
          <h2 class="subFiltro">Gender</h2>
          <input type="checkbox" name="genderFemale" id="genderFemale"><label>Female</label><br>
          <input type="checkbox" name="genderMale" id="genderMale"><label>Male</label><br>
          <input type="checkbox" name="genderGenderless" id="genderGenderless"><label>Genderless</label><br>
          <input type="checkbox" name="genderUnknown" id="genderUnknown"><label>Unknown</label><br>
        </div>
        <div class="filtro">
          <h2 class="subFiltro">Status</h2>
          <input type="checkbox" name="statusAliveAlive" id="statusAliveAlive"><label>Alive</label><br>
          <input type="checkbox" name="statusDead" id="statusDead"><label>Dead</label><br>
          <input type="checkbox" name="statusUnknown" id="statusUnknown"><label>Unknown</label><br>
        </div>
        <div class="filtro">
          <h2 class="subFiltro">Species</h2>
          <input type="checkbox" name="speciesHuman" id="speciesHuman"><label>Human</label><br>
          <input type="checkbox" name="speciesAlien" id="speciesAlien"><label>Alien</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Animal</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Humanoid</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Poopybutthole</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Robot</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Cronenberg</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Disease</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Mythological Creature</label><br>
          <input type="checkbox" name="speciesAnimal" id="speciesAnimal"><label>Unknown</label><br>
        </div>
        <b-button class="botonRosa">Filter</b-button>
      </div>
      <!-- Tabla -->
      <div class="col-10">
        <div>
          <b-table
            striped
            hover
            bordered
            :fields="columnas"
            :items="datos"
            class="custom-table"
          >
            <template #cell(imagenPersonaje)="data">
              <BImg :src="`${data.item.image}`" rounded="circle" />
            </template>
            <template #cell(status)="data">
              {{
                `${data.item.status != "unknown" ? data.item.status : "Unknown"}`
              }}
            </template>
            <template #cell(species)="data">
              {{
                `${data.item.species != "unknown" ? data.item.species : "Unknown"}`
              }}
            </template>
            <template #cell(gender)="data">
              {{
                `${data.item.gender != "unknown" ? data.item.gender : "Unknown"}`
              }}
            </template>
            <template #cell(id)="data">
              <BButton class="botonRosa" @click="send(data.item.id)"
                >Info</BButton
              >
            </template>
          </b-table>
          <div>
            <b-pagination
              v-model="currentPage"
              pills
              :total-rows="rows"
              size="lg"
              align="center"
            ></b-pagination>
          </div>
        </div>
        
      </div> 
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ListaPersonajes",
  data() {
    return {
      datos: null,
      loading: true,
      errored: false,
      currentPage: 1,
      rows: 826,
      columnas: [
        {
          label: "Character",
          key: "imagenPersonaje",
        },
        {
          label: "Name",
          key: "name",
          // sortable: true
        },
        {
          label: "Species",
          key: "species",
          // sortable: true
        },
        {
          label: "Gender",
          key: "gender",
        },
        {
          label: "Status",
          key: "status",
        },
        {
          label: "See more",
          key: "id",
        },
      ],
      filtros: []
    };
  },
  mounted() {
    //se activará después de que se encuntre montado
    this.filtros = [];

    // var baseURL = 'https://rickandmortyapi.com/graphql';
    // var pagInicial = 1;
    this.getPagina();
  },
  watch: {
    currentPage: function () {
      this.getPagina();
      this.scrollToTop();
    },
  },
  methods: {
    send(id) {
      // console.log("El boton presionado: "+id);
      //console.log(page);
      this.$router.push("/listaDetalles/" + id);
    },
    getPagina() {
      var queryVar = `
          query Characters {
              characters(page: ${this.currentPage}) {
                  results {
                      id
                      image
                      name
                      species
                      gender
                      status
                  }
              }
          }
        `;
      //console.log("Query: "+queryVar);
      this.getInfo(queryVar);
    },
    getInfo(queryVar) {
      var baseURL = "https://rickandmortyapi.com/graphql";
      axios
        .post(baseURL, {
          query: queryVar,
        })
        .then((response) => {
          this.datos = response.data.data.characters.results;
          //console.log(response.data.data.characters.results)
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
    scrollToTop() {
      // Función para desplazar al inicio de la página
      window.scrollTo({
        top: 0,
        behavior: 'smooth', // Opcional: hace que el desplazamiento sea suave
      });
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

img {
  width: 160px; /* Ancho deseado */
  height: auto; /* Altura automática para mantener la proporción */
}
.custom-table {
  background-color: rgb(254, 0, 0); /* Cambia lightgray por el color que desees */
}
.b-table tr {
  text-align: center; /* Centrado horizontal */
  vertical-align: middle; /* Centrado vertical */
}
.titulo {
  color: rgb(251, 149, 166);
  font-weight: bold;
  padding: 10px;
  margin: 20px;
  background: rgb(250, 250, 250);
}

.botonRosa {
  background : #fdb4bf !important;
  border: solid 1px #fdb4bf !important;
}

.botonRosa:hover {
  background : #f99aaa !important;
  border: solid 1px #fdb4bf !important;
}

.b-pagination button { /* estilos para todos los botones */
  background-color: #f8f9fa; /* Color de fondo de los botones */
  border-color: #fdb4bf; /* Color del borde del botón activo */
  color: #e57d90; /* Color del texto de los botones */
}

/* Cambiar estilos adicionales */
.b-pagination button:hover {
  color: #ffffff; /* Color del texto de los botones */
  border-color: #fdb4bf; /* Color del borde del botón activo */
  background-color: #fdb4bf; /* Cambiar color de fondo en hover */
  font-weight: bold;
}

.page-item.active .page-link { /* el boton de la página actual */
    color: #fff;
    background-color: #e57d90;
    border-color: #e57d90;
    font-weight: bold;
}

.page-link.active, .active > .page-link {
    border-color: #ed3591;
    background-color: #ed3591; /* Color de fondo del botón activo */
    color: white;
}

.pagination {
    --bs-pagination-focus-box-shadow: 0 0 0 0.25rem #f60021;
}

.subtitulo {
  color: rgb(251, 149, 166);
  font-weight: bold;
  /* padding: 2px; */
  /* margin: 2px; */
  font-size: 25px;
  background: rgb(250, 250, 250);
}

.fondo {
  background: rgb(250, 250, 250);
  padding: 10px;
  margin-left: 20px;
  border-radius: 10px;
}

.filtro {
  background: #ffe4ec;
  text-align: left;
  padding: 8px;
  border-radius: 8px;
  margin-bottom: 10px;
}

.filtro h2 {
  font-size: 15px;
  font-weight: bold;
  margin: 3px;
}

.filtro input {
  font-size: 3px;
  vertical-align: middle; 
  margin: 5px;
}

.filtro label {
  font-size: 13px;
  vertical-align: middle; 
  display: inline;
}

</style>
