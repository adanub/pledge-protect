<template>
  <div class="container">
    <div id="map"><div ref="pins" class="pins-holder"></div></div>
    
    <div class="content-box">
      <app-logo class="app-logo"/>
      
      <h1 class="title">
        <b class="pledge-pink">Pledge</b> & <b class="pledge-pink">Protect</b>
      </h1>
      
      <button  class="dropdown-button" onclick="this.blur();"><dropdown/></button> <!-- Note: onclick="this.blur();" removes focused/selected state after it is pressed, gets rid of the aesthetic issue without compromising accessibility -->
      
      <button class="about-button" onclick="this.blur();">What's this about?</button>

      <div class="button-container">
        <div class="pledge-button-base float-left"><button class="pledge-button" onclick="this.blur();"><pledge-icon class="pledge-button-icon"/><div class="inline-block">Pledge</div></button></div>
        <div class="protect-button-base float-right"><button class="protect-button" onclick="this.blur();"><protect-icon class="protect-button-icon"/><div class="inline-block">&nbsp;Protect</div></button></div>
      </div>

    </div>
  </div>
</template>

<script>
import Dropdown from '~/components/Dropdown.vue'
import AppLogo from '~/components/Logo.vue'
import ProtectIcon from '~/components/ProtectIcon.vue'
import PledgeIcon from '~/components/PledgeIcon.vue'

import ProtectPin from '~/components/ProtectPin.vue'
import PledgePin from '~/components/PledgePin.vue'

import Vue from 'vue'

export default {
  components: {
    Dropdown,
    AppLogo,
    ProtectIcon,
    PledgeIcon,
    PledgePin,
    ProtectPin
  },

  data: () => ({
    map: null,
    pledgePins: [null, null, null],
    protectPins: [null, null, null]
  }),
  methods: {
    //Sets up and starts loading the interactive map
    InitialiseMap() {
      //Max bounds for the map, restricting view to NSW area
      var southWest = L.latLng(-36.959208, 140.929096),
            northEast = L.latLng(-28.246520, 153.628294),
            bounds = L.latLngBounds(southWest, northEast);

      var mapOptions = {
        maxBounds: bounds,
        center: [-33.8688, 151.2093],
        minZoom: 9,
        maxZoom: 14,
        zoomControl: true,
        attributionControl: false
      };

      this.map = this.$L.map('map', mapOptions).fitWorld();
      this.map.zoomControl.setPosition('bottomleft');
      this.$L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', {
          id: 'newuser83/ckhigdse01lyk19qs9k4t7ivh',
          tileSize: 512,
          zoomOffset: -1,
          accessToken: 'pk.eyJ1IjoibmV3dXNlcjgzIiwiYSI6ImNraGlnY3R0ODBhMXMyc3A1ZzY0c3B3OG0ifQ.DyeQQ_lv81eFHSbabPthLw'
      }).addTo(this.map);
      
      this.map.flyTo(new this.$L.LatLng(-33.8688, 151.2093));
      this.map.locate({setView: true});
    },

    //Generates 3 pledge pins, and 3 protect pins at random locations on the screen
    GeneratePins() {
      //Pledge Pins
      for (i = 0; i < 3; i++) {
        var mapBounds = this.map.getBounds();

        //Randomised latitude within view
        var latMagnitude = mapBounds.getEast() - mapBounds.getWest();
        var lat = Math.floor((Math.random() * latMagnitude) + mapBounds.getWest());

        //Randomised longitude within view
        var lngMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        var lng = Math.floor((Math.random() * lngMagnitude) + mapBounds.getSouth());

        var latlng = this.$L.latLng(lat, lng);
        var pixelPosition = this.map.latLngToLayerPoint(latlng);
        console.log("Generated pledge position: " + pixelPosition);
      }

      var PledgeClass = Vue.extend(PledgePin);
      var instance = new PledgeClass({
        propsData: { id: 'pledge1' }
      });
    },

    CreatePledgePin(pinId,  pointX, pointY) {
      var PledgeClass = Vue.extend(PledgePin);
      var instance = new PledgeClass({
        propsData: { 
          id: pinId,
          posX: pointX,
          posY: pointY
        }
      });
      instance.$mount();
      this.$refs.pins.appendChild(instance.$el);

      var mapBounds = this.map.getBounds();
    },

    CreatePledgeMarker(pinId,  pointX, pointY) {
      var PledgeClass = Vue.extend(PledgePin);
      var instance = new PledgeClass({
        propsData: { 
          id: pinId,
          posX: pointX,
          posY: pointY
        }
      });
      instance.$mount();
      this.$refs.pins.appendChild(instance.$el);

      var mapBounds = this.map.getBounds();
    }
  },
  mounted() {
    setTimeout(this.InitialiseMap(), 50); //calls InitialiseMap() 50ms after the page finishes loading
    setTimeout(this.CreatePledgePin("pledge1", 50, 500), 500);
  }
}
</script>

<style>
.pins-holder {
  position: absolute;
  width: 100vw;
  height: 100vh;
  z-index: 405;
}

.pledge-button-icon {
  position: absolute;
  margin-left: auto;
  margin-top: -2px;
}

.protect-button-icon {
  position: absolute;
  margin-left: auto;
  margin-right: ;
  margin-top: -2px;
}

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
  width: 84%;

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

.pledge-button-base {
  background-color: #D64157;
  border-radius: 8px;

  margin-top: 16px;
  margin-left: auto;
  margin-right: auto;
  height: 44px;
  min-width: 135px;
  width: 46.875%;
}
.pledge-button {
  background-color: #F34A63;
  border-radius: 8px;

  color: white;
  font-family: 'Kalam', cursive;
  font-size: 18px;
  padding-top: 6px;
  padding-left: 24px;

  margin-top: -4px;
  height: 44px;
  min-width: 135px;
  width: 100%;

  transition: 0.2s;
}
.pledge-button:hover, .pledge-button:focus {
  outline: none;
  margin-top: -7px;

  transition: 0.1s;
}
.pledge-button:active {
  margin-top: 0px;
  transition: 0.1s;
}

.protect-button-base {
  background-color: #0C8A75;
  border-radius: 8px;

  margin-top: 16px;
  margin-left: auto;
  margin-right: auto;
  height: 44px;
  min-width: 135px;
  width: 46.875%;
}
.protect-button {
  background-color: #0FA78E;
  border-radius: 8px;

  color: white;
  font-family: 'Kalam', cursive;
  font-size: 18px;
  padding-top: 6px;
  padding-left: 24px;

  margin-top: -4px;
  height: 44px;
  min-width: 135px;
  width: 100%;

  transition: 0.2s;
}
.protect-button:hover, .protect-button:focus {
  outline: none;
  margin-top: -7px;

  transition: 0.1s;
}
.protect-button:active {
  margin-top: 0px;
  transition: 0.1s;
}

.button-container {
  margin: auto;
  height: 60px;
  vertical-align: bottom;
  min-width: 288px;
  width: 84%;
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
  z-index: 5;
  border-radius: 16px;
  box-shadow: 0px 4px 16px rgba(0, 0, 0, 0.25);
  top: 16px;
  padding-top: 18px;
  padding-bottom: 18px;
}
</style>
