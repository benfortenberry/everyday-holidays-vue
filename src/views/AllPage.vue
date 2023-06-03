<template>
  <ion-page>
    
    <ion-header>
      <ion-toolbar>
        <ion-title>All Holidays ({{HolidayData.holidays.length}})</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">

       <ion-spinner v-if="loading" name="crescent"></ion-spinner>

      <ion-list v-if="!loading" class='ion-no-padding' >
        <ion-item-group class='border-bottom'
          v-for="h in HolidayData.holidays"
          v-bind:key="h.keyword"
        >
          <ion-item-divider   v-if="!h.date">
            <ion-label  >
              <h1 class='white'>{{ h.name }}</h1>
            </ion-label>
          </ion-item-divider>

          <ion-item v-if="h.date" color='secondary'>
            <ion-label class="ion-text-wrap">
              <h1>{{ h.name }}</h1>
              <p>{{ h.date }}</p>
            </ion-label>
          </ion-item>
        </ion-item-group>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonLabel,
  IonItem,
  IonItemDivider,
  IonItemGroup,
  IonList,
  IonSpinner
} from '@ionic/vue';
import { defineComponent } from 'vue';
import HolidayData from '../../public/assets/json/holidays.json';
export default defineComponent({
  name: 'AllPage',
  data() {
    return {
      term: '',
      HolidayData,
      loading: true,
      plusMinus: 0,
    };
  },
  ionViewDidEnter() {
    this.loading = false;
  },
  methods: {},
  components: {
    IonPage,
    IonItemDivider,
    IonContent,
    IonList,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonLabel,
    IonItem,
    IonItemGroup,
    IonSpinner
  },
});
</script>


<style scoped>
.white {
  color: white;
}

.border-bottom {
  border-bottom: solid #111 1px;
}

</style>