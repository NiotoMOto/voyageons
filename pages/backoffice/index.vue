<template>
 <v-container fluid>
  <v-layout column justify-center align-center>
    <v-dialog v-model="createVoyageDialog" lazy>
      <v-btn primary dark slot="activator">Créer nouveau voyage</v-btn>
      <v-card>
        <v-card-title>
          <span class="headline">Nouveau voyage</span>
        </v-card-title>
        <v-card-text>
          <v-text-field
            label="Nom" @keyup.enter="createVoyage"
            v-model="voyage.name" required>
          </v-text-field>
          <v-text-field
            label="Descriptionn" @keyup.enter="createVoyage"
            v-model="voyage.description"
            required
            multi-line>
          </v-text-field>
          <v-select
            v-bind:items="countries"
            v-model="voyage.country"
            item-text="name"
            item-value="alpha-2"
            label="Pays"
            autocomplete>
          </v-select> 
          <v-checkbox
            label="Publish"
            v-model="voyage.published">
          </v-checkbox>
          <v-checkbox
            label="Active"
            v-model="voyage.active">
          </v-checkbox>

        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn class="blue--text darken-1" flat @click.native="createVoyageDialog = false">Fermer</v-btn>
          <v-btn class="blue--text darken-1" flat @click.native="createVoyage">Création voyage</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-flex xs12>
      <voyage-list :items="items" />
    </v-flex>
  </v-layout>
 </v-container>
</template>

<script>
import axios from 'axios'
import countries from '~/static/countries.json'
import VoyageList from '~/components/backoffice/VoyageList'

export default {
  data: () => ({
    createVoyageDialog: false,
    voyage: {
      name: '',
      country: '',
      published: false,
      active: false
    },
    items: [],
    countries: countries
  }),
  asyncData () {
    return axios.get('http://localhost:4012/api/v1/voyage').then(response => (
      { items: response.data }
    ))
  },
  methods: {
    createVoyage () {
      axios.post('http://localhost:4012/api/v1/voyage', this.voyage).then(data => {
        axios.get('http://localhost:4012/api/v1/voyage').then(response => {
          this.items = response.data
          this.createVoyageDialog = false
          this.voyage = {}
        })
      })
    }
  },
  components: {
    VoyageList
  }
}
</script>
