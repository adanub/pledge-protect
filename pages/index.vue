<template>
  <div class="container">
    <div id="map"></div>
    <div class="content-box">
      <app-logo class="app-logo"/>
      <h1 class="title">
        <b class="pledge-pink">Pledge</b> & <b class="pledge-pink">Protect</b>
      </h1>
      <button  class="dropdown-button" onclick="this.blur();"><dropdown/></button> <!-- Note: onclick="this.blur();" removes focused/selected state after it is pressed, gets rid of the aesthetic issue without compromising accessibility -->
      <button class="about-button" onclick="this.blur();">What's this about?</button>
    </div>
  </div>
</template>

<script>
import Dropdown from '~/components/Dropdown.vue'
import AppLogo from '~/components/Logo.vue'

export default {
  components: {
    Dropdown,
    AppLogo
  },

  data: () => ({
    map: null
  }),
  methods: {
    //Sets up and starts loading the interactive map
    InitialiseMap() {
      var mapOptions = {
        center: [31.2532, 146.9211],
        zoom: 16,
        zoomControl: true,
        attributionControl: false
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
    setTimeout(this.InitialiseMap(), 50); //calls InitialiseMap() 50ms after the page finishes loading
  }
}
</script>

<style>
.app-logo {
  position: absolute;
  left: 16px;
  top: 8px;
}

#map {
  position: absolute;
  height: 100%;
  width: 100vw;
  z-index: 2;

  animation: 2s fade-in;
}

.about-button {
  background-color: #f5f5f5;
  border-width: 2px;
  border-radius: 8px;
  border-color: rgba(59, 65, 60, 0.7);

  color: rgba(59, 65, 60, 0.7);
  font-family: 'Kalam', cursive;
  font-size: 18px;
  padding-top: 6px;

  margin-top: 16px;
  height: 44px;
  min-width: 288px;
  width: 90%;

  transition: 0.1s;
}
.about-button:hover, .about-button:focus {
  outline: none;
  background-color: rgba(59, 65, 60, 1);
  color: #f5f5f5;

  letter-spacing: 0.05rem;
  transition: 0.2s;
}
.about-button:active {
  background-color: rgba(59, 65, 60, 0.9);
  letter-spacing: -0.01rem;
  transition: 0.1s;
}

.dropdown-button {
  position: absolute;
  right: 8px;
  top: 8px;

  transition: 0.2s;
}
.dropdown-button:hover, .dropdown-button:focus {
  outline: none;
  top: 3px;
}

.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-family: 'Heebo', sans-serif;
  color: #3B413C;

  animation: 1s fade-in;
}

@keyframes fade-in {
  0% {
    opacity: 0;
  }
}

.title {
  font-family: 'Kalam', cursive;
  font-size: 24px;
  vertical-align: text-top;
}

.pledge-pink {
  color: #F34A63;
}

.links {
  padding-top: 15px;
}

.content-box {
  display: block;
  position: absolute;
  background-color: #f5f5f5;
  width: 90vw;
  min-height: 200px;
  min-width: 300px;
  max-width: 512px;
  z-index: 3;
  border-radius: 16px;
  box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.25);
  top: 16px;
  padding: 18px;
}
</style>
