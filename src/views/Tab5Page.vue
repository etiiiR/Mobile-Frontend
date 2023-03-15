<script setup lang="ts">
import { computed, reactive } from "vue";
import YAML from "js-yaml";
import { usePermission } from "@vueuse/core";
const accelerometer = usePermission("accelerometer");
const accessibilityEvents = usePermission("accessibility-events");
const ambientLightSensor = usePermission("ambient-light-sensor");
const backgroundSync = usePermission("background-sync");
const camera = usePermission("camera");
const clipboardRead = usePermission("clipboard-read");
const clipboardWrite = usePermission("clipboard-write");
const gyroscope = usePermission("gyroscope");
const magnetometer = usePermission("magnetometer");
const microphone = usePermission("microphone");
const notifications = usePermission("notifications");
const paymentHandler = usePermission("payment-handler");
const persistentStorage = usePermission("persistent-storage");
const push = usePermission("push");
const speaker = usePermission("speaker");
const code = computed(() =>
  YAML.dump(
    reactive({
      accelerometer,
      accessibilityEvents,
      ambientLightSensor,
      backgroundSync,
      camera,
      clipboardRead,
      clipboardWrite,
      gyroscope,
      magnetometer,
      microphone,
      notifications,
      paymentHandler,
      persistentStorage,
      push,
      speaker,
    })
  )
);
</script>

<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Tab 5</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 4</ion-title>
        </ion-toolbar>
      </ion-header>
      <note class="mb-2"> Device Permissions: </note>
      <pre>{{ code }}</pre>
    </ion-content>
  </ion-page>
</template>
