<template>
  <div class="container">
    <div id="map">
       
    </div>
  </div>
</template>

<script>

export default {
  data: () => ({
    map: null
  }),
  methods: {
    InitialiseMap() {
      var mapOptions = {
        center: [31.2532, 146.9211],
        zoom: 16,
        zoomControl: true
      }

      this.map = this.$L.map('map', mapOptions).fitWorld();
      this.map.zoomControl.setPosition('bottomleft');
      this.$L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', {
          maxZoom: 18,
          id: 'newuser83/ckhigdse01lyk19qs9k4t7ivh',
          tileSize: 512,
          zoomOffset: -1,
          accessToken: 'pk.eyJ1IjoibmV3dXNlcjgzIiwiYSI6ImNraGlnY3R0ODBhMXMyc3A1ZzY0c3B3OG0ifQ.DyeQQ_lv81eFHSbabPthLw'
      }).addTo(this.map);
      this.map.flyTo(new this.$L.LatLng(-33.8688, 151.2093), 8);
      this.map.locate({setView: true, maxZoom: 14});
    }
  },
  mounted() {
    setTimeout(this.InitialiseMap(), 50);
  }
}

</script>

<style>
#map {
  position: absolute;
  height: 100%;
  width: 100vw;
  z-index: 2;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.links {
  padding-top: 15px;
}

.content-box {
  display: block;
  background-color: #f2f2f2;
  width: 90vw;
  min-height: 300px;
}

@screen sm {
  .content-box {
    width: 80vw;
    max-width: 600px;
  }
}
</style>
