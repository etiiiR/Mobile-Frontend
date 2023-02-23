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
      <p>{{ motion.x }}{{ motion.y }}{{ motion.z }}</p>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import { Motion } from '@capacitor/motion';


import ExploreContainer from '@/components/ExploreContainer.vue';
const motion = ref({x: 0, y: 0, z: 0})

Motion.addListener('accel', (accel) => {
  console.log('accel', accel);
  motion.value = accel
});

</script>
