<template>
  <div>
    <v-card class="secondary elevation-5">
      <v-card-text>
        <v-container fluid>
          <v-layout row>
            <v-flex xs12>
              <v-text-field
                label="Nom"
                @keyup.enter="updateVoyage"
                v-model="voyage.name" required>
              </v-text-field>
            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12>
              <v-text-field
                label="Descriptionn"
                @keyup.enter="updateVoyage"
                v-model="voyage.description"
                multi-line required>
              </v-text-field>
            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12>
              <v-select
                v-bind:items="countries"
                v-model="voyage.country"
                item-text="name"
                item-value="alpha-2"
                label="Pays"
                autocomplete>
              </v-select>
            </v-flex>
          </v-layout>
          <v-checkbox
            label="Publish"
            v-model="voyage.published">
          </v-checkbox>
          <v-checkbox
            label="Active"
            v-model="voyage.active">
          </v-checkbox>
          <v-layout row class="text-md-center">
            <v-flex class="xs5 offset-xs3">
              <v-btn @click.native="updateVoyage" flat dark primary>Enregistrer</v-btn>
            </v-flex>
          </v-layout>
        </v-container>
      </v-card-text>
    </v-card>
    <v-container fluid>
      <v-layout row>
        <v-flex xs3>
           <v-card class="secondary elevation-3 add-item">
            <v-card-title primary-title>
              <div>
                <div class="headline">Création entrée</div>
              </div>
            </v-card-title>
            <v-card-text>
              <v-container fluid>
                <v-layout row>
                  <v-flex xs12>
                    <upload-image  @change="updateImage" />
                  </v-flex>
                </v-layout>
                <v-layout row class="text-md-center">
                  <v-flex class="xs5 offset-xs3">
                    <v-btn @click.native="addEntry" dark primary>Ajout</v-btn>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card-text>
          </v-card>
        </v-flex>
        <v-flex xs9> 
          <v-card class="secondary elevation-3 add-item">
            <v-card-media :src="image" height="700px">
              <v-container fluid>
                <v-layout row>
                </v-layout>
                </v-container>
            </v-card-media>
         </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios'

import UploadImage from '~/components/UploadImage'
import countries from '~/static/countries.json'

export default {
  data: () => ({
    voyage: {},
    countries: countries,
    id: null,
    image: ''
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
    },
    updateImage (image) {
      console.log('image', image)
      this.image = image
    }
  },
  components: {
    UploadImage
  }
}
</script>

<style>
.add-item {
  margin-top: 30px;
}
</style>