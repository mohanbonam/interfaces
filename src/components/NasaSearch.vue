<template>
    <div>
        <table class="table table-hover table-dark">
  <thead>
    <tr>
      <th scope="col">attitude_quaternions</th>
      <th scope="col">centroid_coordinates</th>
      <th scope="col">identifier</th>
      <th scope="col">image</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="data of nasaData" :key="data">
      <th scope="row">
          <ul v-for="(a, i) in data.attitude_quaternions" :key="a">
              <li>{{i}} {{a}}</li>
          </ul>
          {{data.caption}}
      </th>
      <td>
          <ul  v-for="(a, i) in data.centroid_coordinates" :key="a">
              <li>{{i}}: {{a}}</li>
          </ul>
      </td>
      <td>{{data.identifier}}</td>
      <td>{{data.date}}</td>
    </tr>
  </tbody>
</table>
    </div>
</template>

<script>
export default {
    name: "NasaSearch",
    data() {
        return {
        }
    },
    props: ["nasaData"]
}
</script>

<style scoped>

</style>