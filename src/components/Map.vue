<template>
  <div id="bruh" ref="mapContainer">

  </div>
  <InfoPopup :eventPopup="eventPopup"/>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import L, {icon} from 'leaflet'
import 'leaflet/dist/leaflet.css';
import axios from 'axios';
import {filename} from 'pathe/utils';
import InfoPopup from './InfoPopup.vue';

  const map = ref();
  //allows the div container to be responsive
  const mapContainer = ref();
  const eventPopup = ref({
    eventName: "nope"
  });

  //function is called when component is mounted
  onMounted( async () => {
    //make new map
    map.value = L.map(mapContainer.value).setView([46.069, 11.124], 13.5);
    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 19,
      attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
    }).addTo(map.value);
    //get pins from assets folder
    const allPins = import.meta.glob('@/assets/pins/*.png', { eager: true });
    const images = Object.fromEntries(
      Object.entries(allPins).map(([key, value]) => [filename(key), value.default])
    );
    //get all the available events
    const allEvents = await axios.get('http://localhost:8080/events/all');
    //place pins on map
    createPins(allEvents,images);
  });

  function optionsCreator(tag){
    const urlString = tag;
    var iconOptions = {
      iconUrl: new URL(urlString, import.meta.url).href,
      iconSize: [48,59]
    };
    var customIcon = L.icon(iconOptions);
    var markerOptions = {
      icon: customIcon,
      draggable: false
    }
    return markerOptions;
  }

  function createPins(allEvents,images){
    //parses the events
    const eventList = allEvents.data;
    console.log(images);
    eventList.forEach(element => {
      //gets event coordinates
      const eventCoord = element.eventPosition;
      console.log(element.eventTag);
      const pinColored = optionsCreator(images[element.eventTag]);
      //adds custom options and creates event handler for the InfoPopup component
      const toAdd = L.marker(eventCoord,pinColored).on("click", () => {
        eventPopup.value = element;
      })
      toAdd.addTo(map.value);
      console.log(`created pin for event ${element.eventName}`)
    });
  }

</script>

<style scoped> 
  #bruh{
    height: 100%;
    width: 100%;
  }
</style>