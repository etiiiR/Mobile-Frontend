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
      <note class="mb-2"> Device Orientation: </note>
      <pre lang="json">{{ text }}</pre>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { useGeolocation } from '@vueuse/core'
import { reactive, computed } from "vue";

import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
const geolocation = reactive(useGeolocation());
const text = computed(() => JSON.stringify(geolocation, null, 2));
</script>
