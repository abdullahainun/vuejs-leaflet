<template>
  <div class="container">
    <ul>
      <li
        v-for="(marker, index) in markers"
        :key="index"
        @click="displayTooltip(index);"
      >
        {{ marker.name }}
      </li>
    </ul>

    <l-map :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker
        v-for="(marker, index) in markers"
        :key="index"
        ref="markersRef"
        :lat-lng="marker.position"
      >
        <l-tooltip :content="marker.name" />
        <!-- l-popup :content="marker.name"/ -->
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LTooltip, LPopup } from "vue2-leaflet";
import L from "leaflet";
import "leaflet/dist/leaflet.css";

L.Icon.Default.imagePath = "https://unpkg.com/leaflet@1.3.4/dist/images/";

export default {
  name: "MyMap",
  components: {
    "l-map": LMap,
    "l-tile-layer": LTileLayer,
    "l-marker": LMarker,
    "l-tooltip": LTooltip,
    "l-popup": LPopup
  },
  data() {
    return {
      zoom: 5,
      center: { lat: 59.339025, lng: 18.065818 },
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      markers: [
        { Id: 1, name: "Oslo", position: { lat: 59.923043, lng: 10.752839 } },
        {
          Id: 2,
          name: "Stockholm",
          position: { lat: 59.339025, lng: 18.065818 }
        },
        {
          Id: 3,
          name: "Copenhagen",
          position: { lat: 55.675507, lng: 12.574227 }
        },
        { Id: 4, name: "Berlin", position: { lat: 52.521248, lng: 13.399038 } },
        { Id: 5, name: "Paris", position: { lat: 48.856127, lng: 2.346525 } }
      ],
      markerObjects: null
    };
  },
  mounted: function() {
    L.Icon.Default.imagePath = "https://unpkg.com/leaflet@1.3.4/dist/images/";
    this.$nextTick(() => {
      this.markerObjects = this.$refs.markersRef.map(ref => ref.mapObject);
    });
  },

  methods: {
    displayTooltip(selectedIndex) {
      for (let markerObject of this.markerObjects) {
        markerObject.closeTooltip();
      }
      this.markerObjects[selectedIndex].toggleTooltip();
    }
  }
};
</script>

<style>
li {
  cursor: pointer;
}
</style>
