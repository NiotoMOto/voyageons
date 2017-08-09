<template>

     <v-card class="secondary elevation-0">
    <v-card-text>
      <v-container fluid>
        <v-layout row>
          <v-flex xs4 class="hidden-sm-and-down"> 
            <v-subheader class="grey--text text--lighten-1">Normal with hint text/label</v-subheader>
          </v-flex>
          <v-flex xs8>
            <v-text-field
              label="Nom" @keyup.enter="updateVoyage"
              v-model="voyage.name"
              required
            ></v-text-field>
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs4 class="hidden-sm-and-down">
            <v-subheader class="grey--text text--lighten-1">Focus</v-subheader>
          </v-flex>
          <v-flex xs8>
           <v-text-field
            label="Descriptionn" @keyup.enter="updateVoyage"
            v-model="voyage.description"
            multi-line
            required
          ></v-text-field>
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs4 class="hidden-sm-and-down">
            <v-subheader class="grey--text text--lighten-1">Normal with input text + label</v-subheader>
          </v-flex>
          <v-flex xs8>
            <v-select
              v-bind:items="countries"
              v-model="voyage.country"
              item-text="name"
              item-value="alpha-2"
              label="Pays"
              autocomplete
            ></v-select>
          </v-flex>
        </v-layout>
        <v-layout row class="text-md-center">
          <v-flex class="xs5 offset-xs3">
           <v-btn @click.native="updateVoyage" flat dark primary>Enregistrer</v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-card-text>
</v-card>
</template>

<<script>
import axios from 'axios'

import countries from '~/static/countries.json'

export default {
  data: () => ({
    voyage: {},
    countries: countries,
    id: null
  }),
  asyncData ({ params }) {
    console.log(params)
    return axios.get(`http://localhost:4012/api/v1/voyage/${params.id}`).then(response => (
      {
        voyage: response.data,
        id: params.id
      }
    ))
  },
  methods: {
    updateVoyage () {
      axios.patch(`http://localhost:4012/api/v1/voyage/${this.id}`, this.voyage)
    }
  }
}
</script>

