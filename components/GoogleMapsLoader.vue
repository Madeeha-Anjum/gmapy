<template>
  <section>
    <div ref="googleMap" class="google-map"></div>
    <template v-if="google && map">
      <slot
        :google="google"
        :map="map"
        :marker-img="markerImg"
        :info-window="infoWindow"
      />
    </template>
  </section>
</template>

<script>
import { Loader } from '@googlemaps/js-api-loader'

export default {
  name: 'GoogleMapsLoader',
  props: {
    mapConfig: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      google: null,
      map: null,
      markerImg: null,
      infoWindow: null,
    }
  },
  watch: {
    'mapConfig.center': {
      handler() {
        if (this.map) {
          this.map.setCenter(this.mapConfig.center)
        }
      },
      deep: true,
    },
  },

  async mounted() {
    this.google = await new Loader({
      apiKey: process.env.MAP_API_KEY,
      version: 'weekly',
      libraries: ['places'],
    }).load()
    // Load the map
    this.map = new this.google.maps.Map(this.$refs.googleMap, this.mapConfig)

    // Load marker image
    this.markerImg = new this.google.maps.MarkerImage(
      'pin.svg',
      new this.google.maps.Size(71, 71),
      new this.google.maps.Point(0, 0),
      new this.google.maps.Point(17, 34),
      new this.google.maps.Size(50, 50)
    )
    // Load the info window
    this.infoWindow = new this.google.maps.InfoWindow()
  },
}
</script>
<style>
.google-map {
  grid-area: map;
}
</style>
