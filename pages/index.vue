<template>
  <div class="container">
    <div id="map"><div ref="pins" class="pins-holder"></div></div>
    <floating-menu/>
  </div>
</template>

<script>
import FloatingMenu from '~/components/FloatingMenu.vue'

export default {
  components: {
    FloatingMenu
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
      this.map.fitBounds([
        [-33.818864, 150.973799],
        [-33.960363, 151.352942]
      ])
      this.map.locate({setView: true, maxZoom: 14});
    
      this.map.on('moveend', () => this.GeneratePins());
      console.log("map initialised");
    },

    //Generates 3 pledge pins, and 3 protect pins at random locations on the map within screen view - we don't have a database with actual posts yet, so we're using this to "wizard of oz" the pins.
    GeneratePins() {
      var mapBounds = this.map.getBounds();

      //Pledge Pins
      for (var i = 0; i < 3; i++) {
        var lng = new Object();
        var lat = new Object();

        //Randomised longitude within view
        var lngMagnitude = mapBounds.getEast() - mapBounds.getWest();
        var magnitudeMultiplier = 1.0; //Going to use this to half magnitude to left side of screen if sea is visible

        //Rough guess-timate of whether or not pin spawn area is out at sea based on Sydney
        if (mapBounds.getEast() > 151.698530) {
          magnitudeMultiplier = 0.5;
        } else if (mapBounds.getEast() > 151.486357) {
          magnitudeMultiplier = 0.75;
        }
        lng = (Math.random() * lngMagnitude * 0.26 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 + (lngMagnitude * 0.26 * i * magnitudeMultiplier);
       // lng = (Math.random() * lngMagnitude * 0.8 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 * magnitudeMultiplier;

        //Randomised latitude within view
        var latMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        lat = (Math.random() * latMagnitude * 0.6) + mapBounds.getSouth() + latMagnitude * 0.1;

        this.CreatePledgePin(i, lat, lng);
        //setTimeout(() => this.CreatePledgePin(i, lat, lng), 200 * i);
      }
      //Protect Pins
      for (var i = 0; i < 3; i++) {
        var lng = new Object();
        var lat = new Object();
        
        //Randomised longitude within view
        var lngMagnitude = mapBounds.getEast() - mapBounds.getWest();
        var magnitudeMultiplier = 1.0; //Going to use this to half magnitude to left side of screen if sea is visible
        if (mapBounds.getEast() > 151.352562) {
          magnitudeMultiplier = 0.5;
        }
        lng = (Math.random() * lngMagnitude * 0.26 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 + (lngMagnitude * 0.26 * i * magnitudeMultiplier); //Forced spreading out of pins horizontally
        //lng = (Math.random() * lngMagnitude * 0.8 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 * magnitudeMultiplier; //No forced spreading out horizontally

        //Randomised latitude within view
        var latMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        lat = (Math.random() * latMagnitude * 0.6) + mapBounds.getSouth() + latMagnitude * 0.1;

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
        html: '<a href="" class="pin-marker"><svg width="65" height="86" fill="none" xmlns="http://www.w3.org/2000/svg"><g filter="url(#filter0_d)"><path d="M32.042 52.058c13.27 0 24.028-10.758 24.028-24.03C56.07 14.759 45.312 4 32.042 4 18.77 4 8.012 14.758 8.012 28.029c0 13.27 10.759 24.029 24.03 24.029z" fill="#0FA78E"/><path fill-rule="evenodd" clip-rule="evenodd" d="M32.042 73.992S7.358 43.987 8.012 28.029c.257-6.242 48.146-5.625 48.059-.342-.328 19.685-24.03 46.305-24.03 46.305z" fill="#0FA78E"/><g clip-path="url(#clip0)"><path fill-rule="evenodd" clip-rule="evenodd" d="M32.475 17.005l14.404 5.014s-.472 10.048-2.647 14.379c-3.971 7.906-11.382 9.46-11.757 9.535v.003l-.008-.002-.007.002v-.003c-.376-.074-7.787-1.63-11.758-9.535-2.175-4.33-2.646-14.379-2.646-14.379l14.404-5.014V17l.007.003.008-.003v.005zm7.963 21.696H24.497a7.974 7.974 0 017.97-7.97c4.4 0 7.97 3.571 7.97 7.97zm-7.97-16.693a3.77 3.77 0 013.768 3.77c0 2.08-1.689 3.769-3.769 3.769a3.77 3.77 0 01-3.769-3.77 3.77 3.77 0 013.77-3.769z" fill="#fff"/></g></g><defs><clipPath id="clip0"><path fill="#fff" transform="translate(18 17)" d="M0 0h29v29H0z"/></clipPath><filter id="filter0_d" x="0" y="0" width="64.071" height="85.992" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB"><feFlood flood-opacity="0" result="BackgroundImageFix"/><feColorMatrix in="SourceAlpha" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0"/><feOffset dy="4"/><feGaussianBlur stdDeviation="4"/><feColorMatrix values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0.25 0"/><feBlend in2="BackgroundImageFix" result="effect1_dropShadow"/><feBlend in="SourceGraphic" in2="effect1_dropShadow" result="shape"/></filter></defs></svg></a>'
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
  }
}
</script>

<style>
.leaflet-div-icon {
  background: transparent;
  border: none;
  outline: none;
  overflow: visible;
}

.pin-marker {
  z-index: inherit;
  position: absolute;
  width: 65px;
  height: 86px;
  left: -32.5px;
  top: -74px;
  transition: all 0.2s cubic-bezier(0.47, 1.64, .41, .8);
  overflow: visible;

  animation: 0.3s popup1;
}

@keyframes popup1 {
  0% {
    transform: scale(0);
    top: 0px;
  }
  50% {
    transform: scale(1.3);
    top: -85px;
  }
  100% {
    transform: scale(1);
    top: -74px;
  }
}

.pin-marker:hover, .pin-marker:focus {
  outline: none;
  top: -85px;
  transition: all 0.3s cubic-bezier(0.47, 2.64, .41, .8);
  transform: scale(1.3);
}

.pins-holder {
  position: absolute;
  width: 0;
  height: 0;
  z-index: 405;
}

#map {
  position: absolute;
  height: 100%;
  width: 100vw;
  z-index: 2;

  animation: 2s fade-in;
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
</style>
