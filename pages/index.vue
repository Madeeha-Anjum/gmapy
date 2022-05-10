<template>
  <section>
    <GoogleMapsLoader :map-config="mapConfig" class="loader-sect">
      <template v-slot="{ google, map, markerImg, infoWindow }">
        <!--  Autocomplete -->
        <GoogleMapsAutocomplete
          class="search-sect"
          :google="google"
          :map="map"
          @place-search="onPlaceSearch"
        />

        <!-- Markers  -->
        <GoogleMapsMarker
          v-for="point in points"
          :key="point.id"
          :marker-img="markerImg"
          :google="google"
          :map="map"
          :point="point"
          :open-info-window="selectedPointCard === point.id"
          :info-window="infoWindow"
          @marker-clicked="onMarkerClick"
        />

        <section class="points-sect scrollable">
          <PointCard
            v-for="point in points"
            :key="point.id"
            :ref="'element' + point.id"
            :point="point"
            @click.native="onPointCard(point.id)"
          />
        </section>
      </template>
    </GoogleMapsLoader>
  </section>
</template>

<script>
import GoogleMapsLoader from '@/components/GoogleMapsLoader.vue'
import GoogleMapsMarker from '@/components/GoogleMapsMarker.vue'
import GoogleMapsAutocomplete from '@/components/GoogleMapsAutocomplete.vue'
import PointCard from '@/components/PointCard.vue'
export default {
  name: 'IndexPage',
  components: {
    GoogleMapsLoader,
    GoogleMapsMarker,
    GoogleMapsAutocomplete,
    PointCard,
  },
  data() {
    return {
      selectedPointCard: null,
      mapConfig: {
        center: {
          lat: -34.397,
          lng: 150.644,
        },
        zoom: 15,
      },
      points: [
        {
          id: 1,
          place: {
            name: 'Restaurant 1',
            address: 'Address of restaurant 1',
            description: 'Restaurant 1 is the most populous place',
          },
          position: {
            long: -123.102959,
            lat: 49.282589,
          },
        },
        {
          id: 2,
          place: {
            name: 'Restaurant 2',
            address: 'Address of restaurant 2',
            description: 'Restaurant 2 is the most populous place',
          },
          position: {
            long: -123.063354,
            lat: 49.249123,
          },
        },
        {
          id: 3,
          place: {
            name: 'Restaurant 3',
            address: 'Address of restaurant 3',
            description: 'Restaurant 3 is the most populous place',
          },
          position: {
            long: -123.024897,
            lat: 49.282012,
          },
        },
        {
          id: 4,
          place: {
            name: 'Restaurant 4',
            address: 'Address of restaurant 4',
            description: 'Restaurant 4 is the most populous place',
          },
          position: {
            long: -123.094797,
            lat: 49.282918,
          },
        },
        {
          id: 5,
          place: {
            name: 'Restaurant 5',
            address: 'Address of restaurant 5',
            description: 'Restaurant 5 is the most populous place',
          },
          position: {
            long: -123.194898,
            lat: 49.282718,
          },
        },
      ],
    }
  },

  async created() {
    this.mapConfig.center = await this.getLocation()
  },

  methods: {
    onMarkerClick(point) {
      this.$refs[`element${point.id}`][0].$el.scrollIntoView({
        behavior: 'smooth',
        block: 'start',
      })
    },

    onPointCard(pointId) {
      // Find the selectedPoint with the given id
      const selectedPoint = this.points.find((point) => point.id === pointId)

      // use the selectedPoint to set the map center
      this.mapConfig.center = {
        lat: selectedPoint.position.lat,
        lng: selectedPoint.position.long,
      }

      this.selectedPointCard = pointId
    },

    onPlaceSearch(place) {
      //  update center
      this.mapConfig.center = {
        lat: place.geometry.location.lat(),
        lng: place.geometry.location.lng(),
      }
    },

    getLocation() {
      if (process.client) {
        return new Promise((resolve, reject) => {
          if (!('geolocation' in navigator)) {
            reject(new Error('Geolocation is not available.'))
          }
          navigator.geolocation.getCurrentPosition(
            (position) => {
              resolve({
                lat: position.coords.latitude,
                lng: position.coords.longitude,
              })
            },
            (err) => {
              reject(err)
            }
          )
        })
      }
    },
  },
}
</script>

<style>
/* ===== Layout  ===== */
.loader-sect {
  display: grid;
  grid-template-rows: 6em 1fr 1fr;
  height: 500px;
  grid-template-areas:
    'autocomplete autocomplete autocomplete'
    'points map map'
    'points map map';
}

.search-sect {
  grid-area: autocomplete;
  width: 100%;
  display: flex;
  padding: 2em 1em;
  margin: 0 auto;
}

.points-sect {
  grid-area: points;
}

/* =====  End Layout   ===== */

/*  Source: https://codeconvey.com/custom-scrollbar-css-for-all-browsers/ */
.scrollable {
  overflow: scroll;
}
/* scrollbar width  */
.scrollable::-webkit-scrollbar {
  width: 10px;
}
/* scrollbar Track */
.scrollable::-webkit-scrollbar-track {
  background: white;
}

/* scrollbar Handle */
.scrollable::-webkit-scrollbar-thumb {
  background: #f2f2f2;
  border-radius: 20px;
}
/* scrollbar Handle on hover */
.scrollable::-webkit-scrollbar-thumb:hover {
  background: #f2f2f6;
}
</style>
