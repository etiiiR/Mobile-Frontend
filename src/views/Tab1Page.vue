<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>

      <ExploreContainer name="Tab 1 page" />
      <h1 @click="checkpermission">Click here</h1>
      <p>acceleration: {{ acceleration.x }}{{ acceleration.y }}{{ acceleration.z }}</p>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from "@ionic/vue";
import { Motion } from "@capacitor/motion";
import { PluginListenerHandle } from "@capacitor/core";
import ExploreContainer from "@/components/ExploreContainer.vue";
const acceleration = ref({ x: 0, y: 0, z: 0 });

const checkpermission = async () => {
  console.log('clicked')
  console.log(acceleration.value)
  try {
    await DeviceMotionEvent.requestPermission();
  } catch (e) {
    // Handle error
    return
  }
};
// add the listener to Motion with the callback and the permission check of the sensor
const accelHandler = await Motion.addListener("accel", (event) => {
  acceleration.value = event.acceleration;
});





</script>
