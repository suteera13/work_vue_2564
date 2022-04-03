<template>
  <v-row>
    <v-col
      cols="12"
      sm="6"
      offset-sm="3"
    >
      <v-card height="100px">
        <v-card-title class="bgcard white--text">
          <span class="text-h5">Menu</span>

          <v-spacer />

          <v-menu
            bottom
            left
          >
            <template #activator="{ on, attrs }">
              <v-btn
                dark
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item
                v-for="(item, i) in items"
                :key="i"
              >
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-card-title>

        <v-card-text />
      </v-card>
      <Nuxt />
      <v-card
        class="mx-auto overflow-hidden"
        height="200"
        max-width="500"
      >
        <v-bottom-navigation
          absolute
          color="white"
          hide-on-scroll
          horizontal
          scroll-target="#scroll-threshold-example"
          scroll-threshold="500"
        >
          <v-btn>
            <span>Recents</span>

            <v-icon>mdi-history</v-icon>
          </v-btn>

          <v-btn>
            <span>Favorites</span>

            <v-icon>mdi-heart</v-icon>
          </v-btn>

          <v-btn>
            <span>Nearby</span>

            <v-icon>mdi-map-marker</v-icon>
          </v-btn>
        </v-bottom-navigation>

        <v-sheet
          id="scroll-threshold-example"
          class="overflow-y-auto pb-16"
          max-height="600"
        >
          <v-responsive height="1500" />
        </v-sheet>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data: () => ({
    items: [
      { title: 'Click Me' },
      { title: 'Click Me' },
      { title: 'Click Me' },
      { title: 'Click Me 2' }
    ]
  })
}
</script>

<style>
  .bgcard{
    background: #0989ff;
  }
</style>
