<template>
  <v-list three-line>
    <template v-for="item in items" >
      <v-subheader v-if="item.header" v-text="item.header">
        </span>
      </v-subheader>
      <v-divider v-else-if="item.divider" v-bind:inset="item.inset"></v-divider>
      <v-list-tile avatar v-else v-bind:key="item.title"  router :to="`/backoffice/voyages/${item._id}`">
        <v-list-tile-avatar>
          <span v-bind:class="`flag-icon flag-icon-${item.country ? item.country.toLowerCase() : ''}`"></span>
        </v-list-tile-avatar>
        <v-list-tile-content>
          <v-list-tile-title v-html="item.name"></v-list-tile-title>
          <v-list-tile-sub-title v-html="item.description"></v-list-tile-sub-title>
          <v-icon v-if="item.published" light class="green--text text--darken-2">check circle</v-icon>
        </v-list-tile-content>
      </v-list-tile>
    </template>
  </v-list>
</template>

<script>
export default {
  props: ['items']
}
</script>
