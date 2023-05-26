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
      <ion-button @click="predictData">Predict activity</ion-button>
      <ion-button @click="clearData">Clear your Acitvity</ion-button>
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
import { ref, watch } from "vue";

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
} from "@ionic/vue";
import { useDeviceMotion, useDeviceOrientation } from "@vueuse/core";
import { reactive, computed, onMounted } from "vue";
import { useGeolocation } from "@vueuse/core";
import { Storage } from "@ionic/storage";

const orientation = reactive(useDeviceOrientation());
const textOrientation = computed(() => JSON.stringify(orientation, null, 2));
const store = new Storage();
await store.create();

//const accelerometer = usePermission("accelerometer");
//const magnetometer = usePermission("magnetometer");
//const gyroscope = usePermission("gyroscope");
const motion = reactive(useDeviceMotion());

const textMotion = computed(() => JSON.stringify(motion, null, 2));
const measurments = ref([]);
watch(
  () => ({
    alpha: orientation.alpha,
    beta: orientation.beta,
    gamma: orientation.gamma,
    x: motion.acceleration?.x,
    y: motion.acceleration?.y,
    z: motion.acceleration?.z,
    gx: motion.accelerationIncludingGravity?.x,
    gy: motion.accelerationIncludingGravity?.y,
    gz: motion.accelerationIncludingGravity?.z,
    rx: motion.rotationRate?.alpha,
    ry: motion.rotationRate?.beta,
    rz: motion.rotationRate?.gamma,
  }),
  (value) => {
    if (value.alpha == null) return;
    if (value.beta == null) return;
    if (value.gamma == null) return;
    const epoch = new Date("1970-01-01");
    const datetime = new Date();
    const seconds = (datetime - epoch) / 1000;
    const milliseconds = Math.floor(seconds * 1000);
    value["time"] = milliseconds;
    measurments.value.push(value);
  }
);

const createData = () => {
  const window_size = 400;
  const step_size = 100;
  debugger;
  const X = measurments.value.map((value) => [
    value.x || 0,
    value.y || 0,
    value.z || 0,
    value.rx || 0,
    value.ry || 0,
    value.rz || 0,
    value.gx || 0,
    value.gy || 0,
    value.gz || 0,
    value.alpha || 0,
    value.beta || 0,
    value.gamma || 0,
  ]);
debugger
  // Create a sliding window of X with the specified window and step sizes
  const X_windows = [];
  for (let i = 0; i <= X.length - window_size; i += step_size) {
    const window = X.slice(i, i + window_size);
    X_windows.push(window);
  }
  // Reshape X_windows to 3D format (samples, timesteps, features)
  const samples = X_windows.length;
  try {
    const timesteps = X_windows[0].length;
    const features = X_windows[0][0].length;
    const reshapedX_windows = tf
      .tensor(X_windows)
      .reshape([samples, timesteps, features]);
    return reshapedX_windows;
  } catch (err) {
    alert("Not enough data to make Windows to predict");
  }
};

const { coords, locatedAt, error } = useGeolocation();
await store.set("key", textMotion.value);
const modelLoaded = ref(false);

const importModel = async () => {
  const model = await tf.loadLayersModel("model.json");
  modelLoaded.value = true;
  return model;
};

const clearData = () => {
  measurments.value = [];
};

const predictData = async () => {
  const model = await importModel();
  const X_widow = createData();
  debugger;
  const values = await X_widow.array();

  const res = await model.predict(X_widow);
  alert(res);
  // [laufen, rennen, sitzen, stehen, treppenlaufen, velofahren]
  const labels = [
    "laufen",
    "rennen",
    "sitzen",
    "stehen",
    "treppenlaufen",
    "velofahren",
  ];
  debugger;
  //const index = res.argMax(1).dataSync()[0];
  const predictions = res.argMax(1).dataSync();

  // Count the occurrences of each predicted value
  const counts = {};
  predictions.forEach((value) => {
    if (counts[value]) {
      counts[value]++;
    } else {
      counts[value] = 1;
    }
  });

  // Find the value with the maximum count
  let maxCount = 0;
  let mostCountedValue;

  Object.keys(counts).forEach((value) => {
    if (counts[value] > maxCount) {
      maxCount = counts[value];
      mostCountedValue = parseInt(value); // Convert value to an integer
    }
  });

  console.log("Most counted value:", mostCountedValue);

  const label = labels[mostCountedValue];
  alert(label);
};

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
