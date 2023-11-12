<template>
  <v-layout class="rounded rounded-md">
    <!--:order="order"-->
    <v-app-bar color="grey-lighten-2" flat title="BUSCADOR DE PELICULAS">
    </v-app-bar>

    <v-app-bar flat>
      <v-sheet width="800" class="mx-auto">
        <br>
        <br>
        <v-form @submit.prevent>
          <div class="d-flex align-rigth">
            <!--:rules="rules"--> <br>
            <v-text-field v-model="nombre" label="Nombre película" class="pa-3" outlined></v-text-field>
            <v-btn class="mt-6" @click="BuscarBoton()">
              Buscar
            </v-btn>
          </div>
          <br>
        </v-form>
      </v-sheet>
    </v-app-bar>


    <v-main class="d-block align-center justify-center" style="min-height: auto;">
      <v-container>
        <v-row no-gutters>
          <v-col  style="height:auto;" cols="4" v-if="mostrar" v-for="pelicula in peli">
            <br>
            <v-card  class="mx-auto" max-width="344">
              <v-img :src="pelicula.Poster" height="500" alter="no encontrado" cover=""></v-img>

              <v-card-text>
                {{ pelicula.Title }}
              </v-card-text>

              <v-card-subtitle>
                {{ pelicula.Year }}
              </v-card-subtitle>


              <v-card-actions>
                <v-btn variant="text" color="teal-accent-4" @click="BuscarID(pelicula.imdbID)">
                  Ver Más
                </v-btn>
              </v-card-actions>

              <v-expand-transition>
                <v-card v-if="MostrarMas &&  pelicula.imdbID==this.idBuscar"  class="v-card--reveal" style="height: 100%;">

                  <v-card-text class="pb-0">  
                    <p class="text-h5 text--primary">
                      {{ pelicula.Title }}
                    </p>
                     <p class="text-h6">Descripción:</p> {{this.info.Plot }}

                     <p class="text-h6">Valoraciones:</p>

                     <p v-for="infor in this.info.Ratings">
                      {{ infor.Source }}: {{ infor.Value }}
                    </p> 
                  </v-card-text>

                  <v-card-actions class="pt-0">
                    <v-btn variant="text" color="teal-accent-4" @click="this.MostrarMas = false && this.idBuscar==0">
                      Cerrar
                    </v-btn>
                  </v-card-actions>

                </v-card>
              </v-expand-transition>

            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-layout>
</template>

<script>

import { buscar,buscarID } from './../services/conexion'

export default {
  data() {
    return {
      reveal: false,
      mostrar: false,
      MostrarMas : false,
      nombre: "",
      peli: [],
      info: [],
      idBuscar: 0
    };
  },

  methods: {
    BuscarBoton() {
      buscar(this.nombre).then((response) => {
        this.peli = response;
        console.log(this.peli);
      });
      this.mostrar = true;
    },

    BuscarID(id){
      buscarID(id).then((response)=>{
        this.idBuscar = id;
        this.info = response;
        console.log(response);
      })
      this.idBuscar=0;
      this.MostrarMas=true;
    },
  },

  created() {
    console.log("CREADO");
  },

  updated() {
    console.log("UPDATE");
  }
};
</script>