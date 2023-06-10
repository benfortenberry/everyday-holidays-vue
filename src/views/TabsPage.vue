<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom">
        <ion-tab-button tab="home" href="/app/home">
          <ion-icon :icon="calendar" />
          <ion-label>Today</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="search" href="/app/search">
          <ion-icon :icon="search" />
          <ion-label>Search</ion-label>
        </ion-tab-button>

        <ion-tab-button tab="all" href="/app/all">
          <ion-icon :icon="list" />
          <ion-label>All</ion-label>
        </ion-tab-button>
        <ion-tab-button tab="settings" href="/app/settings">
          <ion-icon :icon="notifications" />
          <ion-label>Notify</ion-label>
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonTabBar,
  IonTabButton,
  IonTabs,
  IonLabel,
  IonIcon,
  IonPage,
  IonRouterOutlet,
} from '@ionic/vue';
import { calendar, search, list, notifications } from 'ionicons/icons';
</script>
