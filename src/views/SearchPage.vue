<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Search</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <ion-row>
          <ion-col>
            <ion-item>
              <ion-input
                label="Find your holiday: "
                @ionInput="doSearch"
                v-model="term"
              ></ion-input>
            </ion-item>
          </ion-col>
        </ion-row>
      </ion-grid>
      <p v-if="term" class="ion-padding-start">
        Holidays found: {{ searchData.length }}
      </p>

      <ion-accordion-group v-if="term">
        <ion-accordion
          v-for="h in searchData"
          v-bind:key="h.keyword"
          :value="h.keyword"
        >
          <ion-item slot="header" color="tertiary">
            <ion-label>{{ h.name }} - {{ h.date }}</ion-label>
          </ion-item>
          <div class="border-top" slot="content">
            <ion-card class="ion-no-margin ">
              <img :src="`assets/imgs/${h.keyword}.jpg`" />

              <ion-card-content class="white ">
                {{ h.blurb }}
              </ion-card-content>
              <ion-button @click="doShare(h.name, h.date)" fill="clear"
                ><ion-icon color="dark" :icon="shareSocial"></ion-icon
              ></ion-button>
            </ion-card>
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
  IonInput,
  IonCol,
  IonRow,
  IonCardContent,
  IonAccordion,
  IonAccordionGroup,
  IonGrid,
  IonCard,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import { shareSocial } from 'ionicons/icons';
import HolidayData from '../../public/assets/json/holidays.json';
import { Share } from '@capacitor/share';
export default defineComponent({
  name: 'SearchPage',
  data() {
    return {
      term: '',
      HolidayData,
      searchData: [{}],
      shareSocial,
      plusMinus: 0,
    };
  },

  methods: {
    async doShare(holidayName: any, date: any) {
      try {
        await Share.share({
          text: date + ' is ' + holidayName,

          dialogTitle: 'Share holiday',
        });
        console.log(holidayName);
      } catch {
        console.log('error');
      }
    },
    doSearch() {
      console.log('do search');
      this.searchData.length = 0;
      this.HolidayData.holidays.forEach((it) => {
        if (it.name.toLowerCase().includes(this.term) && it.date) {
          this.searchData.push(it);
        }
      });
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
    IonCardContent,
    IonLabel,
    IonItem,
    IonAccordion,
    IonAccordionGroup,
    IonInput,
    IonCard,
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
