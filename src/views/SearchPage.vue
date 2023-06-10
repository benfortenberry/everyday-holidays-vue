<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title> Search</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col>
            <ion-item>
              <ion-input
                placeholder="Find your Holiday"
                v-model="term"
                :debounce="500"
                @ionInput="doSearch"
              />
            </ion-item>
          </ion-col>
        </ion-row>
      </ion-grid>
      <p v-if="term && term.length > 1" class="ion-padding-start">
        Holidays found: {{ searchData.length }}
      </p>
      <ion-accordion-group v-if="term && term.length > 1">
        <ion-accordion
          v-for="h in searchData"
          v-bind:key="h.id"
          :value="h.keyword"
        >
          <ion-item slot="header" color="tertiary">
            <ion-label>{{ h.name }} - {{ h.date }}</ion-label>
          </ion-item>
          <div class="border-top" slot="content">
            <holiday-card :holidayData="[h]" />
          </div>
        </ion-accordion>
      </ion-accordion-group>
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
  IonCol,
  IonRow,
  IonAccordion,
  IonAccordionGroup,
  IonGrid,
  IonInput,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import HolidayData from '../../public/assets/json/holidays.json';
import HolidayCard from '../components/HolidayCard.vue';

export default defineComponent({
  name: 'SearchPage',
  data() {
    return {
      term: '',
      HolidayData,
      searchData: [{}],
    };
  },

  methods: {
    doSearch() {
      if (this.term.length > 1) {
        this.searchData.length = 0;
        this.HolidayData.holidays.forEach((it) => {
          if (it.name.toLowerCase().includes(this.term) && it.date) {
            this.searchData.push(it);
          }
        });
      }
    },
  },
  components: {
    IonPage,
    IonCol,
    IonContent,
    IonRow,
    IonGrid,
    IonTitle,
    IonHeader,
    IonToolbar,
    IonLabel,
    IonItem,
    IonAccordion,
    IonAccordionGroup,
    HolidayCard,
    IonInput,
  },
});
</script>

<style scoped>
.white {
  color: white;
}

.border-top {
  border-top: solid #111 1px;
}

.border-bottom {
  border-bottom: solid #111 1px;
}
</style>
