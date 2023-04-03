<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1.1</ion-title>
      </ion-toolbar>
    </ion-header>

      <ion-content class="ion-padding">
                        
        <ion-item-group>
            <ion-item class="list-header" lines="none">
                <ion-label class="header-label" slot="start"><strong>General Information</strong></ion-label>
            </ion-item>
            <ion-item lines="none">
                <ion-label style="padding-left: 6px;" position="floating">Select Area starting from Entrance</ion-label>                
                <ion-select v-model="area" ref="sel" @IonChange="onIonChangeSelect($event)">
                  <ion-select-option value="pantry">Pantry</ion-select-option>
                  <ion-select-option value="familyRoom">Family Room</ion-select-option>
                </ion-select>
            </ion-item>
            <!-- :options="{minZoom: -50,fadeAnimation:true}" -->
            <!-- options="{maxZoom: 0,fadeAnimation:true}" -->
            <!-- :options="{maxZoom: 0.2,fadeAnimation:true}" -->
        </ion-item-group>
        <div></div>
        <l-map
          ref="map"
          v-model:zoom="zoom"
          :center="[147.41322, 510.219482]"          
          :options="{fadeAnimation:true}"
          :crs="crs"
        >
          <l-image-overlay
            :url="url"
            :bounds="bounds"
          />
          <div v-if="area=='pantry'">
            <l-marker 
              v-for="star in pt"
              :key="star.name"
              :lat-lng="star"            
            >
              <l-popup :content="star.name" />
            </l-marker>
            <l-polyline :opacity="0.5" :lat-lngs="polyline.pantry"></l-polyline>
          </div> 

          <div v-if="area=='familyRoom'">
            <l-marker 
              v-for="star in fr"
              :key="star.name"
              :lat-lng="star"            
            >
              <l-popup :content="star.name" />
            </l-marker>
            <l-polyline :opacity="0.5" :lat-lngs="polyline.familyRoom"></l-polyline>
          </div>      
        </l-map>
      
      <!-- <div id="map"></div>   -->
      </ion-content>
  </ion-page>
</template>

<script>
  import L from "leaflet";  
  import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonSelect, IonSelectOption } from '@ionic/vue';
  import "leaflet/dist/leaflet.css";
  import { LMap, LTileLayer,LImageOverlay, LMarker, LPopup, LPolyline } from "@vue-leaflet/vue-leaflet";

  import { ref } from 'vue'
  export default {
    components: {
      LMap,
      LImageOverlay,
      LMarker,
      LPopup,
      LPolyline,
      IonPage,
      IonSelect, 
      IonSelectOption,
    },
    /* created() {
      
    }, */
    data() {
      return {
        area: null,
        targetArea: '',
        url: "../../assets/sample-floorplan-main.jpg",
        //bounds: [[-26.5, -25], [1021.5, 1023]],
        bounds: [[-26.5, -25], [500.5, 1023]],
        //zoom: -150,
        zoom: -150,
        crs: L.CRS.Simple,
        pt: [
          { name: "Cafeteria", lng: 730.2, lat: 315.0 },
          /* { name: "Mizar", lng: 41.6, lat: 130.1 },
          { name: "Krueger-Z", lng: 13.4, lat: 56.5 }, */
          { name: "Entrance", lng: 318.7, lat: 8.3 },
        ],
        fr: [
          { name: "Entrance", lng: 318.7, lat: 8.3 },
          { name: "Family Room", lng: 330.0, lat:340.2 }
        ],
        polyline: {
          pantry: [
              [18.3, 318.7],
              [170.3, 311.7],
              [170.0, 450.2], 
              [280.3, 448.7],
              [280.0, 570.2], 
              [307.0, 570.2],  
              [307.0, 725.2],       
          ],
          familyRoom: [
              [18.3, 318.7],
              [170.3, 311.7],
              [170.0, 450.2], 
              [350.3, 448.7],
              [350.0, 330.2],  
          ],
          color: "orange",
          className: 'animate'
        },
      };
    },
    mounted() {
      this.initMap();
    },
    methods: {
      onIonChangeSelect(value){
        this.area = value.detail.value
      },
      initMap() {
        // create a new map object
       // const map = L.map('map');

        // set the initial map view and zoom level
        //map.setView([147.41322, 510.219482], 1);

        // add an image overlay to the map
        /* const imageUrl = "../../assets/sample-floorplan-main.jpg";
        const imageBounds = [[0, 0], [1000, 1000]];
        L.imageOverlay(imageUrl, imageBounds).addTo(map); */
      },
    },
  };

</script>

<style>
  .leaflet-interactive {
    stroke-dasharray: 800;
    stroke-dashoffset: 0;
    animation: dash 5s infinite;
  }

  @keyframes dash {
    from {
      stroke-dashoffset: 822;
    }
    to {
      stroke-dashoffset: 0;
    }
  }

  #map {
    height: 500px;
  }

  @media screen and (max-width: 768px) {
    #map {
      height: 300px;
    }
  }

  @media screen and (max-width: 576px) {
    #map {
      height: 200px;
    }
  }

  .image-layer {
    width: 100%;
    height: 100%;
  }
  
</style>