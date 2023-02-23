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

      <ion-button @click="StopMeasure">{{ ButtonText }}</ion-button>
      <p>acceleration: {{ acceleration.x }}{{ acceleration.y }}{{ acceleration.z }}</p>
      <p>
        accelerationIncludingGravity: {{ accelerationIncludingGravity.x
        }}{{ accelerationIncludingGravity.y }}{{ accelerationIncludingGravity.z }}
      </p>
      <p>
        orientation: {{ orientation.alpha }}{{ orientation.beta }}{{ orientation.gamma }}
      </p>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonButton, alertController } from "@ionic/vue";
import { ref, onMounted } from "vue";
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from "@ionic/vue";
import { Motion } from "@capacitor/motion";
import { PluginListenerHandle } from "@capacitor/core";
import ExploreContainer from "@/components/ExploreContainer.vue";
const acceleration = ref({ x: 0, y: 0, z: 0 });
const accelerationIncludingGravity = ref({ x: 0, y: 0, z: 0 });
const orientation = ref({ alpha: 0, beta: 0, gamma: 0 });
const ButtonText = ref("Start Measure");

const checkpermission = async () => {
  console.log("clicked");
  console.log(acceleration.value);
  try {
    await DeviceMotionEvent.requestPermission();
  } catch (e) {
    // Handle error
    presentAlert();
    return;
  }
};
// add the listener to Motion with the callback and the permission check of the sensor
const accelHandler = await Motion.addListener("accel", (event) => {
  acceleration.value = event.acceleration;
  accelerationIncludingGravity.value = event.accelerationIncludingGravity;
});

const rotationHandler = await Motion.addListener("orientation", (event) => {
  orientation.value = event;
});

const StopMeasure = async () => {
  await Motion.removeAllListeners();
  if (ButtonText.value == "Start Measure") {
    checkpermission();
    ButtonText.value = "Stop Measure";
  } else {
    ButtonText.value = "Start Measure";
  }
};

const presentAlert = async () => {
  const alert = await alertController.create({
    header: "Alert",
    subHeader: "Error",
    message: "This is an alert!",
    buttons: ["OK"],
  });

  await alert.present();
};
</script>
