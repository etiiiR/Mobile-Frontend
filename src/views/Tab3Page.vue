<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Tab 3</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 3</ion-title>
        </ion-toolbar>
      </ion-header>
      <note class="mb-2"> Device Orientation: </note>
      <pre lang="json">{{ text }}</pre>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { useDeviceOrientation } from '@vueuse/core'
import { reactive, computed } from "vue";

import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
const orientation = reactive(useDeviceOrientation());
const text = computed(() => JSON.stringify(orientation, null, 2));
</script>
