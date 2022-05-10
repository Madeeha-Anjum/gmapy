<template>
  <section class="card-sect">
    <div class="card">
      <h1>{{ point.place.name }}</h1>
      <p>{{ point.place.address }}</p>
      <p>{{ point.place.description }}</p>
    </div>
  </section>
</template>

<script>
export default {
  name: 'PointsCard',
  props: {
    point: {
      type: Object,
      required: true,
    },
  },
}
</script>
<style scoped>
.card-sect {
  padding: 1em;
}
.card {
  border: 1px solid black;
}
</style>
