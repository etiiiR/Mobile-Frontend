<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar >
        <ion-title>Tab 1</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-button @click="checkpermission" >Allow Motion Permission</ion-button>
      <note class="mb-2"> Device Motion: </note>
      <pre lang="json">{{ textMotion }}</pre>
      <note class="mb-2"> Device Orientation: </note>
      <pre lang="json">{{ textOrientation }}</pre>
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
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import * as tf from '@tensorflow/tfjs';
import { IonButton, alertController } from "@ionic/vue";
import { ref } from "vue";
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from "@ionic/vue";
import { Motion } from "@capacitor/motion";
import { useDeviceMotion, usePermission, useDeviceOrientation } from "@vueuse/core";
import { reactive, computed, onMounted } from "vue";
import { useGeolocation } from "@vueuse/core";
import { Dialog } from '@capacitor/dialog';

const ButtonText = ref("Start Measure");
const permission = ref(false);

const orientation = reactive(useDeviceOrientation());
const textOrientation = computed(() => JSON.stringify(orientation, null, 2));


const accelerometer = usePermission("accelerometer");
const magnetometer = usePermission("magnetometer");
const gyroscope = usePermission("gyroscope");

const motion = reactive(useDeviceMotion());
const textMotion = computed(() => JSON.stringify(motion, null, 2));

const { coords, locatedAt, error, resume, pause } = useGeolocation();

const importModel = async () => {
  const TensorflowModel = await fetch(
    "https://raw.githubusercontent.com/ahmedkhalil1998/Model/main/model.json"
  ).then((res) => res.json());

  const model = await tf.loadLayersModel(`data:${TensorflowModel}`);
  return model
};

const predict = async (model: any, data: any) => {
  const prediction = await model.predict(data);
  return prediction;
};

const predictData = async () => {
  const model = await importModel();
  const data = tf.tensor([[
    3434
  ]]);
  const prediction = await predict(model, data);
  return prediction;
};

const checkpermission = async () => {
  try {
    await DeviceMotionEvent.requestPermission();
    //await DeviceOrientationEvent.requestPermission();
  } catch (e) {
    // Handle error
    presentAlert(e.message);
    return;
  }
};


const presentAlert = async (m: string) => {
  const alert = await alertController.create({
    header: "Alert",
    subHeader: "Error",
    message: m,
    buttons: ["OK"],
  });

  await alert.present();
};

onMounted(async () => {
  await checkpermission();
});


</script>
