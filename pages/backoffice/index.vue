<template>
  <v-layout column justify-center align-center>
     <v-dialog v-model="createVoyageDialog" lazy>
      <v-btn primary dark slot="activator">Créer nouveau voyage</v-btn>
      <v-card>
        <v-card-title>
          <span class="headline">Nouveau voyage</span>
        </v-card-title>
        <v-card-text>
          <v-text-field label="Email" @keyup.enter="createVoyage" v-model="voyageName" required></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn class="blue--text darken-1" flat @click.native="createVoyageDialog = false">Fermer</v-btn>
          <v-btn class="blue--text darken-1" flat @click.native="createVoyage">Création voyage</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
import axios from 'axios'

export default {
  data: () => ({
    createVoyageDialog: true,
    voyageName: ''
  }),
  methods: {
    createVoyage () {
      console.log(this)
      axios.post('http://localhost:4012/api/v1/voyage', { name: this.voyageName }).then(data => {
        axios.get('http://localhost:4012/api/v1/voyage').then(data => {
          console.log(data)
          this.createVoyageDialog = false
        })
      })
    }
  }
}
</script>
