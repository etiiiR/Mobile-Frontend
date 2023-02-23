<template>
  <ion-app>
    <Suspense>
      <ion-router-outlet />
    </Suspense>
    <template #fallback> Loading... </template>
  </ion-app>
</template>

<script setup lang="ts">
import { IonApp, IonRouterOutlet } from "@ionic/vue";
</script>
