<template>
  <v-container fluid>
    <v-row>
      <v-col cols="1"></v-col>
      <v-col cols="10">
        <v-row >
          <v-layout :wrap="true">
            <v-flex xs12>
              <v-card>
                <v-date-picker
                  
                  color="rgb(43,46,59)"
                  v-model="fecha"
                  full-width
                  class="mt-4"
                  locale="es-cl"
                  :min="minimo"
                  :max="max"
                  @change="getDolar(fecha)"
                ></v-date-picker>
              </v-card>
              <v-card color="secondary" dark>
                <v-card-text class="title text-center">{{ fecha }} - {{ valor }}</v-card-text>
              </v-card>
            </v-flex>
          </v-layout>
        </v-row>
      </v-col>
        <v-col cols="1"></v-col>
    </v-row>
  </v-container>
</template>

<script>
// @ is an alias to /src
import axios from "axios";
export default {
  data() {
    return {
      alignmentsAvailable: ["start", "center", "end", "baseline", "stretch"],
      alignment: "center",
      dense: false,
      justifyAvailable: [
        "start",
        "center",
        "end",
        "space-around",
        "space-between"
      ],
      justify: "center",
      fecha: new Date().toISOString().substr(0, 10),
      minimo: "1984",
      max: new Date().toISOString().substr(0, 10),
      valor: null
    };
  },
  methods: {
    async getDolar(dia) {
      console.log(dia);
      let arrayFecha = dia.split(["-"]);
      console.log(arrayFecha);
      let ddmmaa = arrayFecha[2] + "-" + arrayFecha[1] + "-" + arrayFecha[0];
      console.log(ddmmaa);

      try {
        let datos = await axios.get(
          `https://mindicador.cl/api/dolar/${ddmmaa}`
        );
        if (datos.data.serie.length > 0) {
          this.valor = await datos.data.serie[0].valor;
        } else {
          this.valor = "sin resultados";
        }
      } catch (error) {
        // console.log(err);
      } finally {
      }
    }
  },
  created() {
    this.getDolar(this.fecha);
  }
};
</script>
