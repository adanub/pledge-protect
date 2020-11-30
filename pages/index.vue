<template>
  <div class="container">
    <div id="map" class="disable-select">
    <div ref="pins" class="pins-holder"></div></div>
    <floating-menu/>
    <pledge-post class="modal-window"/>
    <protect-post class="modal-window"/>
    <pledge-form class="modal-window"/>
  </div>
</template>

<script>
import FloatingMenu from '~/components/FloatingMenu.vue'

import PledgePost from '~/components/PledgePost.vue'
import ProtectPost from '~/components/ProtectPost.vue'

import PledgePin from '~/components/Buttons/PledgePin.vue'
import ProtectPin from '~/components/Buttons/ProtectPin.vue'
import Vue from 'vue' //Needed for runtime instantiation of components (map pins)

import PledgeForm from '~/components/PledgeForm/1.vue'

export default {
  components: {
    FloatingMenu,
    PledgePost,
    ProtectPost,
    PledgePin,
    ProtectPin,
    PledgeForm
  },

  data: () => ({
    //-----------------------------------------Map and Pins------------------------------------------
    map: null,
    pinMarkers: new Array(6),

    //-------------------------------------Pledge/Protect Posts-------------------------------------
    allPosts: [ //A placeholder as there is currently no database to dynamically get post data from
      { //Post 1
        name: "Jared",
        location: "Western Sydney",
        who: "This is my dad, Jared. He loves his veggie garden, and tends to it every morning, rain or shine. He's the most caring person I know, and I really want to help him stay safe during this pandemic!",
        why: "According to official information from NSW Health, Jared is vulnerable to COVID-19 due to his age and having chronic medical conditions, and he will likely fall seriously ill if he catches it."
      },
      { //Post 2
        name: "Doan",
        location: "Western Sydney",
        who: "My dad was a refugee who fled Vietnam by boat at the end of the Vietnam War. He migrated entirely to a new country with barely any grasp on English, money or resources. He has been working hard for decades to provide a better life for my sisters and me.  He gave me the world. I want the chance to give him the world back. But I am worried about what could happen to my dad if he catches COVID because of his old and health conditions. Please help me protect my dad!",
        why: "According to official information from NSW Health, Doan is vulnerable to COVID-19 as he is over 65 with a chronic medical condition. He is likely to fall seriously ill if he catches the virus."
      },
      { //Post 3
        name: "Susan",
        location: "Newcastle",
        who: "My mum is an awesome woman that has lived around the world, explored many different careers, and always tried to do the best for me and my sibling. She loves being out in nature and has an energy to life that is difficult to rival. I’m worried that she may catch COVID while shopping or at a park and have a pretty bad time from it. I would really appreciate if you can help keep my mum safe.",
        why: "According to official information from NSW Health, Susan is vulnerable to COVID-19 as she has a chronic medical condition. She is moderately likely to fall seriously ill if she catches the virus."
      }
    ]

    //-------------------------------------Pledge/Protect Forms-------------------------------------
    

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
          magnitudeMultiplier = 0.68;
        }
        lng = (Math.random() * lngMagnitude * 0.8/3 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 + (lngMagnitude * 0.8/3 * i * magnitudeMultiplier);
       // lng = (Math.random() * lngMagnitude * 0.8 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 * magnitudeMultiplier;

        //Randomised latitude within view
        var latMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        lat = (Math.random() * latMagnitude * 0.3) + mapBounds.getSouth() + latMagnitude * 0.1;
        this.CreatePledgePin(i, lat, lng);
        //setTimeout(() => this.CreatePledgePin(i, lat, lng), 200 * i);
      }
      //Protect Pins
      for (var i = 0; i < 3; i++) {
        var lng = new Object();
        var lat = new Object();
        
        //Randomised longitude within view
        var lngMagnitude = mapBounds.getEast() - mapBounds.getWest();
        var magnitudeMultiplier = 1.0; //Going to use this to half magnitude to left side of screen if right side of screen is far out to sea
        if (mapBounds.getEast() > 151.352562) {
          magnitudeMultiplier = 0.5;
        }
        lng = (Math.random() * lngMagnitude * 0.8/3 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 + (lngMagnitude * 0.8/3 * i * magnitudeMultiplier); //Forced spreading out of pins horizontally
        //lng = (Math.random() * lngMagnitude * 0.8 * magnitudeMultiplier) + mapBounds.getWest() + lngMagnitude * 0.1 * magnitudeMultiplier; //No forced spreading out horizontally

        //Randomised latitude within view
        var latMagnitude = mapBounds.getNorth() - mapBounds.getSouth();
        var topPadding = (screen.height - 500) / screen.height; //Percentage of free vertical space for pins without getting under the floating menu
        lat = (Math.random() * latMagnitude * topPadding) + mapBounds.getSouth() + latMagnitude * 0.1;

        this.CreateProtectPin(i + 3, lat, lng);
      }
    },

    CreatePledgePin(pinId,  lat, lng) {
      var refId = "pledgepin" + pinId; //Will use this to find the marker generated by Leaflet and replace it with a proper nuxt-compatible version

      //Creating custom marker
      var pledgeIcon = this.$L.divIcon({
        iconSize: null,
        html: '<div id="' + refId + '">'
      });
      
      var marker = this.$L.marker([lat, lng], {icon: pledgeIcon}).addTo(this.map);
      if (this.pinMarkers[pinId] != null) {
        this.map.removeLayer(this.pinMarkers[pinId]);
      }
      this.pinMarkers[pinId] = marker;

      //Creating a vue instance of pin to replace leaflet's marker with
      var ComponentClass = Vue.extend(PledgePin);
      var instance = new ComponentClass() //Can optionally pass props in here for future features
      var mapInstance = document.getElementById(refId);
      if (mapInstance != null) {
        instance.$mount(mapInstance);
      } else {
        console.log("Map instance of pin " + refId + " not found.");
      }
    },

    CreateProtectPin(pinId,  lat, lng) {
      var refId = "protectpin" + pinId; //Will use this to find the marker generated by Leaflet and replace it with a proper nuxt-compatible version

      //Creating custom marker
      var protectIcon = this.$L.divIcon({
        iconSize: null,
        html: '<div id="' + refId + '">'
      });
      
      var marker = this.$L.marker([lat, lng], {icon: protectIcon}).addTo(this.map);
      if (this.pinMarkers[pinId] != null) {
        this.map.removeLayer(this.pinMarkers[pinId]);
      }
      this.pinMarkers[pinId] = marker;

      //Creating a vue instance of pin to replace leaflet's marker with
      var ComponentClass = Vue.extend(ProtectPin);
      var instance = new ComponentClass() //Can optionally pass props in here for future features
      var mapInstance = document.getElementById(refId);
      if (mapInstance != null) {
        instance.$mount(mapInstance);
      } else {
        console.log("Map instance of pin " + refId + " not found.");
      }
    }
  },
  mounted() {
    setTimeout(this.InitialiseMap(), 50); //calls InitialiseMap() 50ms after the page finishes loading
  }
}
</script>

<style>
.modal-window {
  position: absolute;
  left: 0;

  z-index: 5000;
}

.leaflet-div-icon {
  background: none;
  border: none;
  outline: none;
}

.pin-marker {
  z-index: inherit;
  position: absolute;
  width: 65px;
  height: 86px;
  left: -32.5px;
  top: -74px;
  transition: all 0.2s cubic-bezier(0.47, 1.64, .41, .8);

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
  top: 0;
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
  overflow: hidden;
}

@keyframes fade-in {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.title {
  font-family: 'Kalam', cursive;
  font-weight: 700;
  font-size: 24px;
  vertical-align: text-top;
}

.pledge-pink {
  color: #F34A63;
}
.protect-green {
  color: #0FA78E;
}
.text-grey {
  color: #3B413C;
}

.links {
  padding-top: 15px;
}

.disable-select {
    user-select: none; /* supported by Chrome and Opera */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
}
</style>
