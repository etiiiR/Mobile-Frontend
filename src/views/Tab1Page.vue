<template>
  <ion-page>
    <ion-header :translucent="true">
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
      <ion-button @click="checkpermission">Allow Motion Permission</ion-button>
      <br />
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
      <note class="mb-2"> Storage: </note>
      <pre>{{ store.get("key") }}</pre>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import * as tf from "@tensorflow/tfjs";

import { IonButton, alertController } from "@ionic/vue";
import { ref } from "vue";



import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from "@ionic/vue";
import { Motion } from "@capacitor/motion";
import {
  useDeviceMotion,
  usePermission,
  useDeviceOrientation,
} from "@vueuse/core";
import { reactive, computed, onMounted } from "vue";
import { useGeolocation } from "@vueuse/core";
import { Storage } from "@ionic/storage";

const ButtonText = ref("Start Measure");
const permission = ref(false);

const orientation = reactive(useDeviceOrientation());
const textOrientation = computed(() => JSON.stringify(orientation, null, 2));
const store = new Storage();
await store.create();

const accelerometer = usePermission("accelerometer");
const magnetometer = usePermission("magnetometer");
const gyroscope = usePermission("gyroscope");
const motion = reactive(useDeviceMotion());

const textMotion = computed(() => JSON.stringify(motion, null, 2));

const { coords, locatedAt, error, resume, pause } = useGeolocation();
await store.set("key", textMotion.value);
const modelLoaded = ref(false);
const model = ref();

const importModel = async () => {
  const response = await fetch("model.json");
  const model = await response.json();
  modelLoaded.value = true;
  return model;
};

const predict = async (model: any, data: any) => {
  const prediction = await model.predict(data);
  return prediction;
};

const predictData = async () => {
  const model = await importModel();
  const window_size = 400;
  const timestep = 100;
  // create a tensor like time,Accelerometer_x,Accelerometer_y,Accelerometer_z,Gyroscope_x,Gyroscope_y,Gyroscope_z,Magnetometer_x,Magnetometer_y,Magnetometer_z,Orientation_qx,Orientation_qy,Orientation_qz
  //const { alpha, beta, gamma } = useDeviceOrientation();
  //const { acceleration, accelerationIncludingGravity, rotationRate } =
   // useDeviceMotion();

  // Get the current datetime in milliseconds
  //const interval = Date.now() * 1000;
  //const n_features = 13;
  //const data = [
  //  interval, // time
  //  acceleration.value?.x, // Accelerometer_x
  //  acceleration.value?.y, // Accelerometer_y
  //  acceleration.value?.z, // Accelerometer_z
  //  rotationRate.value?.alpha, // Gyroscope_x
  //  rotationRate.value?.beta, // Gyroscope_y
  //  rotationRate.value?.gamma, // Gyroscope_z
  //  accelerationIncludingGravity.value?.x, // Magnetometer_x
  //  accelerationIncludingGravity.value?.y, // Magnetometer_y
  //  accelerationIncludingGravity.value?.z, // Magnetometer_z
  //  alpha, // Orientation_qx
  //  beta, // Orientation_qy
  //  gamma, // Orientation_qz
  //];
  //const tensorData = tf.tensor(data, [1, window_size, n_features]);
  //const prediction = await predict(model, tensorData);
  //console.log(prediction);
  
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
  debugger;
  predictData();
});
</script>
