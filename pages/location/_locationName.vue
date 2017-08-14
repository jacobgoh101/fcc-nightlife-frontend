<template>
  <v-layout row wrap>
    <v-flex v-for="result in results" :key="result.id" xs12 sm6 md4 class="mb-4">
      <v-card>
        <v-card-media :src="getImage(result)" height="200px">
        </v-card-media>
        <v-card-title primary-title>
          <div>
            <h3 class="headline mb-0">{{result.name}}</h3>
            <div>{{result.formatted_address}}</div>
          </div>
        </v-card-title>
        <v-card-actions>
          <v-btn flat class="orange--text">Share</v-btn>
          <v-btn flat class="orange--text">Explore</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios';

export default {
  computed: {
    locationName () { return this.$route.params.locationName }
  },
  methods: {
    getImage (result) {
      let photoReference = '';
      try {
        photoReference = result.photos[0].photo_reference;
      } catch (e) { }
      if (!photoReference) return `http://i.imgur.com/Tf04xmm.jpg`;
      return `https://maps.googleapis.com/maps/api/place/photo?key=AIzaSyDeKjNwKDJTosG35VBa2KgL_bQAr8Sk7sU&photoreference=${photoReference}&maxheight=400`;
    }
  },
  components: {
  },
  async asyncData ({ params }) {
    const locationName = params.locationName
    let { data } = await axios.get(`https://maps.googleapis.com/maps/api/place/textsearch/json?key=AIzaSyDeKjNwKDJTosG35VBa2KgL_bQAr8Sk7sU&query=${encodeURI("bars in " + locationName)}`);
    let { results } = data;
    return { results };
  }
}
</script>

<style>

</style>