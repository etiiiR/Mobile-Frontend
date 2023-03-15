<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Tab 4</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 4</ion-title>
        </ion-toolbar>
      </ion-header>
      <note class="mb-2"> Device Geolocation: </note>
      <pre lang="json">{{
        JSON.stringify(
          {
            coords: {
              accuracy: coords.accuracy,
              latitude: coords.latitude,
              longitude: coords.longitude,
              altitude: coords.altitude,
              altitudeAccuracy: coords.altitudeAccuracy,
              heading: coords.heading,
              speed: coords.speed,
            },
            locatedAt,
            error: error ? error.message : error,
          },
          null,
          2
        )
      }}</pre>
      <ion-button @click="resume">resume</ion-button>
      <ion-button @click="pause">pause</ion-button>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { useGeolocation } from "@vueuse/core";
import { reactive, computed } from "vue";

import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from "@ionic/vue";
import ExploreContainer from "@/components/ExploreContainer.vue";
const { coords, locatedAt, error, resume, pause } = useGeolocation();
</script>
