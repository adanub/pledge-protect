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

import Vue from 'vue'

export default {
  components: {
    Dropdown,
    AppLogo,
    ProtectIcon,
    PledgeIcon
  },

  data: () => ({
    map: null,
    pinMarkers: new Array(6)
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

      this.map.on('moveend', () => this.GeneratePins());
      console.log("map initialised");
    },

    //Generates 3 pledge pins, and 3 protect pins at random locations on the screen
    GeneratePins() {
      var mapBounds = this.map.getBounds();

      //Pledge Pins
      for (var i = 0; i < 3; i++) {
        //Randomised longitude within view
        var lngMagnitude = mapBounds.getEast() - mapBounds.getWest();
        var magnitudeMultiplier = 1.0; //Going to use this to half magnitude to left side of screen if sea is visible
        //Rough guess-timate of whether or not pin spawn area is out at sea based on Sydney
        if (mapBounds.getEast() > 151.698530) {
          magnitudeMultiplier = 0.5;
        } else if (mapBounds.getEast() > 151.486357) {
          magnitudeMultiplier = 0.75;
        }
        var lng = (Math.random() * lngMagnitude * 0.8 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 * magnitudeMultiplier;

        //Randomised latitude within view
        var latMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        var lat = (Math.random() * latMagnitude * 0.6) + mapBounds.getSouth() + latMagnitude * 0.1;

        this.CreatePledgePin(i, lat, lng);
      }
      //Protect Pins
      for (var i = 0; i < 3; i++) {
        //Randomised longitude within view
        var lngMagnitude = mapBounds.getEast() - mapBounds.getWest();
        var magnitudeMultiplier = 1.0; //Going to use this to half magnitude to left side of screen if sea is visible
        if (mapBounds.getEast() > 151.352562) {
          magnitudeMultiplier = 0.5;
        }
        //var lng = (Math.random() * lngMagnitude * 0.26 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 + (lngMagnitude * 0.26 * i * magnitudeMultiplier);
        var lng = (Math.random() * lngMagnitude * 0.8 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 * magnitudeMultiplier;

        //Randomised latitude within view
        var latMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        var lat = (Math.random() * latMagnitude * 0.6) + mapBounds.getSouth() + latMagnitude * 0.1;

        this.CreateProtectPin(i + 3, lat, lng);
      }
    },

    CreatePledgePin(pinId,  lat, lng) {
      //Creating custom marker
      var pledgeIcon = this.$L.divIcon({
        iconSize: null,
        html: '<a href="" class="pin-marker"> <svg width="65" height="86" viewBox="0 0 65 86" fill="none" xmlns="http://www.w3.org/2000/svg"> <g filter="url(#filter0_d)"> <path d="M32.0416 52.0577C45.3123 52.0577 56.0704 41.2996 56.0704 28.0289C56.0704 14.7581 45.3123 4 32.0416 4C18.7708 4 8.0127 14.7581 8.0127 28.0289C8.0127 41.2996 18.7708 52.0577 32.0416 52.0577Z" fill="#F34A63"/> <path fill-rule="evenodd" clip-rule="evenodd" d="M32.0417 73.9916C32.0417 73.9916 7.35805 43.9873 8.0128 28.0294C8.26887 21.7873 56.1584 22.4045 56.0705 27.6867C55.7432 47.3719 32.0417 73.9916 32.0417 73.9916Z" fill="#F34A63"/> <g clip-path="url(#clip0)"> <path fill-rule="evenodd" clip-rule="evenodd" d="M20.1842 40.8169C19.9955 40.3638 19.9431 39.841 20.0675 39.3232L22.3972 29.6214C22.6556 28.5453 23.5908 27.8512 24.5827 27.9328H34.4876C35.4019 27.9328 36.2423 28.1971 36.9024 28.7085C37.0236 28.7608 37.1424 28.8248 37.2576 28.9006L40.8067 31.2365L40.3371 24.2012C40.26 23.0461 41.0658 22.0407 42.1354 21.9575C43.205 21.8741 44.1359 22.7444 44.2129 23.8995L44.9719 35.2713C45.0081 35.8133 44.8499 36.3221 44.5614 36.7185C43.8783 37.9207 42.4204 38.3008 41.3042 37.5662L38.3326 35.6103V40.8169H26.2302L33.4901 35.9119C34.6004 35.1618 34.9379 33.5792 34.2434 32.3801C33.5488 31.181 32.0835 30.8165 30.9732 31.5667L25.3458 35.3686L25.1598 36.1433L31.2605 32.1199C32.0899 31.573 33.1745 31.856 33.6809 32.7518C34.1874 33.6475 33.9253 34.819 33.0958 35.3659L24.8303 40.8169H20.1842ZM30.7288 14.0659C34.0262 14.0659 36.7034 16.9572 36.7034 20.5185C36.7034 24.0798 34.0262 26.9711 30.7288 26.9711C27.4313 26.9711 24.7541 24.0798 24.7541 20.5185C24.7541 16.9572 27.4313 14.0659 30.7288 14.0659Z" fill="white"/> </g> </g> <defs> <filter id="filter0_d" x="0" y="0" width="64.0707" height="85.9916" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB"> <feFlood flood-opacity="0" result="BackgroundImageFix"/> <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"/> <feOffset dy="4"/> <feGaussianBlur stdDeviation="4"/> <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"/> <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/> <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/> </filter> <clipPath id="clip0"> <rect width="25" height="27" fill="white" transform="translate(20 14)"/> </clipPath> </defs> </svg> </a>'
      });
      
      var marker = this.$L.marker([lat, lng], {icon: pledgeIcon}).addTo(this.map);
      if (this.pinMarkers[pinId] != null) {
        this.map.removeLayer(this.pinMarkers[pinId]);
      }
      this.pinMarkers[pinId] = marker;
    },

    CreateProtectPin(pinId,  lat, lng) {
      //Creating custom marker
      var protectIcon = this.$L.divIcon({
        iconSize: null,
        html: '<a href="" class="pin-marker"><svg width="65" height="86" viewBox="0 0 65 86" fill="none" xmlns="http://www.w3.org/2000/svg"> <g filter="url(#filter0_d)"> <path d="M32.0416 52.0577C45.3123 52.0577 56.0704 41.2996 56.0704 28.0289C56.0704 14.7581 45.3123 4 32.0416 4C18.7708 4 8.0127 14.7581 8.0127 28.0289C8.0127 41.2996 18.7708 52.0577 32.0416 52.0577Z" fill="#0FA78E"/> <path fill-rule="evenodd" clip-rule="evenodd" d="M32.0417 73.9916C32.0417 73.9916 7.35805 43.9873 8.0128 28.0294C8.26887 21.7873 56.1584 22.4045 56.0705 27.6867C55.7432 47.3719 32.0417 73.9916 32.0417 73.9916Z" fill="#0FA78E"/> <g clip-path="url(#clip0)"> <path fill-rule="evenodd" clip-rule="evenodd" d="M32.4746 17.0051L46.8785 22.0192C46.8785 22.0192 46.4073 32.0672 44.232 36.3979C40.2609 44.3039 32.8499 45.8587 32.4746 45.9329V45.9358L32.4671 45.9343L32.4597 45.9358V45.9329C32.0843 45.8587 24.6735 44.3039 20.7022 36.3979C18.5269 32.0672 18.0557 22.0192 18.0557 22.0192L32.4597 17.0051V17L32.4671 17.0026L32.4746 17V17.0051ZM40.4378 38.701H24.4966C24.4966 34.302 28.068 30.7305 32.4671 30.7305C36.8662 30.7305 40.4378 34.302 40.4378 38.701ZM32.4671 22.0084C34.5474 22.0084 36.2363 23.6973 36.2363 25.7775C36.2363 27.8578 34.5474 29.5467 32.4671 29.5467C30.3868 29.5467 28.698 27.8578 28.698 25.7775C28.698 23.6973 30.3868 22.0084 32.4671 22.0084Z" fill="white"/> </g> </g> <defs> <filter id="filter0_d" x="0" y="0" width="64.0707" height="85.9916" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB"> <feFlood flood-opacity="0" result="BackgroundImageFix"/> <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"/> <feOffset dy="4"/> <feGaussianBlur stdDeviation="4"/> <feColorMatrix type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"/> <feBlend mode="normal" in2="BackgroundImageFix" result="effect1_dropShadow"/> <feBlend mode="normal" in="SourceGraphic" in2="effect1_dropShadow" result="shape"/> </filter> <clipPath id="clip0"> <rect width="29" height="29" fill="white" transform="translate(18 17)"/> </clipPath> </defs> </svg></a>'
      });
      
      var marker = this.$L.marker([lat, lng], {icon: protectIcon}).addTo(this.map);
      if (this.pinMarkers[pinId] != null) {
        this.map.removeLayer(this.pinMarkers[pinId]);
      }
      this.pinMarkers[pinId] = marker;
    }
  },
  mounted() {
    setTimeout(this.InitialiseMap(), 50); //calls InitialiseMap() 50ms after the page finishes loading
    //setTimeout(() => this.GeneratePins(), 500);
    //window.setInterval(() => this.GeneratePins(), 3000);
  }
}
</script>

<style>
.leaflet-div-icon {
  background: none;
  border: none;
  z-index: 430;
  overflow: visible;
  position: absolute;
  outline: none;
}

.pin-marker {
  position: absolute;
  width: 65px;
  height: 86px;
  left: -32.5px;
  top: -74px;
  display: block;
  transition: 0.2s;
}

.pin-marker:hover, .pin-marker:focus {
  outline: none;
  top: -85px;
  transition: 0.3s;
}

.pins-holder {
  position: absolute;
  width: 0;
  height: 0;
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
