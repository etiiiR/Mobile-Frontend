<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Tab 2</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 2</ion-title>
        </ion-toolbar>
      </ion-header>
      <note class="mb-2"> Store: </note>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { Storage } from '@ionic/storage';
import { ref } from 'vue';
const store = new Storage();
await store.create();
await store.set('key', 'value');
const text = ref('');
await store.get('key').then((value) => {
  text.value = value;
});
</script>
