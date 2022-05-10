<!-- eslint-disable  vue/valid-template-root -->
<template></template>
<script>
export default {
  name: 'GoogleMapsMarker',
  props: {
    google: {
      type: Object,
      required: true,
    },
    map: {
      type: Object,
      required: true,
    },
    markerImg: {
      type: Object,
      required: true,
    },
    infoWindow: {
      type: Object,
      required: true,
    },
    point: {
      type: Object,
      required: true,
    },
    openInfoWindow: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      marker: null,
    }
  },
  watch: {
    openInfoWindow() {
      if (this.openInfoWindow) {
        this.updateInfoWindow()
      }
    },
  },

  beforeDestroy() {
    this.marker.setMap(null)
  },

  mounted() {
    this.marker = new this.google.maps.Marker({
      position: {
        lat: this.point.position.lat,
        lng: this.point.position.long,
      },
      icon: this.markerImg,
      map: this.map,
    })

    // 1. Create a listener on the marker
    this.marker.addListener('click', () => {
      this.updateInfoWindow()
      // send event to the parent component
      this.$emit('marker-clicked', this.point)
    })
  },

  methods: {
    updateInfoWindow() {
      this.infoWindow.setContent(
        `
        <div>
        <img src="pin.svg" style="width:25px; " />
        <h1>${this.point.place.name}</h1>
        <p>${this.point.place.address}</p>
        <p>${this.point.place.description}</p>

       </div>
        `
      )

      this.infoWindow.open({
        anchor: this.marker,
        map: this.map,
        shouldFocus: false,
      })
    },
  },
}
</script>
