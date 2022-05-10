<template>
  <section>
    <input ref="inputRef" class="input" placeholder="Enter address" />
    <a-button type="primary" @click="onPlaceSearch">
      Search
      <a-icon type="search" />
    </a-button>
  </section>
</template>

<script>
export default {
  name: 'GoogleMapsAutoComplete',
  props: {
    google: {
      type: Object,
      required: true,
    },
    map: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      autocomplete: null,
    }
  },
  mounted() {
    this.autocomplete = new this.google.maps.places.Autocomplete(
      this.$refs.inputRef
    )
    // Bias the results to be places close to what you can see on the map
    this.autocomplete.bindTo('bounds', this.map)
  },
  methods: {
    onPlaceSearch() {
      const place = this.autocomplete.getPlace()
      this.$emit('place-search', place)
    },
  },
}
</script>

<style scoped>
.input {
  width: 100%;
  height: 34px;
  border-radius: 5px;
  border: 1px solid #ccc;
  padding: 0 10px;
}
</style>
