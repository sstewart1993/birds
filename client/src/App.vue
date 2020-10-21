<template>
  <div id="app">
    <sightings-form />
    <sightings-grid :sightings="sightings" />
  </div>
</template>

<script>
import {eventBus} from '@/main'
import SightingsForm from './components/SightingsForm';
import SightingsGrid from './components/SightingsGrid';
import SightingService from './services/SightingService.js';

export default {
  name: 'app',
  components: {
    'sightings-form': SightingsForm,
    'sightings-grid': SightingsGrid
  },
  data() {
    return {
      sightings: []
    };
  },
	mounted() {
    this.fetchSightings();
  
	SightingService.getSightings()
	.then(sightings => this.sightings = sightings)

	eventBus.$on('sighting-added', (payload) => {
    // console.log('payload', payload);
    this.sightings.push(payload)
  })
  },
  methods: {
    fetchSightings() {
      SightingService.getSightings()
        .then(sightings => this.sightings = sightings);
    },
    addBird(fly){
			fly.preventDefault()
			const sighting = {
				date: this.date,
				location: this.location,
				species: this.species,
			}
		SightingServive.postSighting(sighting)
		.then(sighting => eventBus.$emit('sighting-added', sighting))
		}
  }
}
</script>

<style>
html {
  height: 100%;
}

body {
  background: url('./assets/birds-background.jpg') no-repeat;
  height: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;

}
</style>
