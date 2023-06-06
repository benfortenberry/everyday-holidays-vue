<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>All Holidays ({{ HolidayData.holidays.length }})</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-list class="ion-no-padding">
        <ion-item-group
          class="border-bottom"
          v-for="h in renderData"
          v-bind:key="h.id"
        >
          <ion-item-divider v-if="!h.date">
            <ion-label>
              <h2 class="white">{{ h.name }}</h2>
            </ion-label>
          </ion-item-divider>

          <ion-item v-if="h.date" color="secondary">
            <ion-label class="ion-text-wrap">
              <h2>{{ h.name }}</h2>
              <p>{{ h.date }}</p>
            </ion-label>

            <share-button :holiday="h" />

            <notify-button :holiday="h" />
          </ion-item>
        </ion-item-group>
      </ion-list>
      <ion-infinite-scroll @ionInfinite="ionInfinite">
        <ion-infinite-scroll-content></ion-infinite-scroll-content>
      </ion-infinite-scroll>
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
  IonInfiniteScroll,
  InfiniteScrollCustomEvent,
  IonInfiniteScrollContent,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import HolidayData from '../../public/assets/json/holidays.json';
import NotifyButton from '../components/NotifyButton.vue';
import ShareButton from '../components/ShareButton.vue';

export default defineComponent({
  name: 'AllPage',
  data() {
    return {
      HolidayData,
      renderData: [{}],
      renderMonth: 'January',
    };
  },
  ionViewDidEnter() {
    this.renderData.length = 0;
    HolidayData.holidays.forEach((h) => {
      if (
        h.name == this.renderMonth ||
        h.date.toString().includes(this.renderMonth)
      ) {
        this.renderData.push(h);
      }
    });
  },
  ionViewDidLeave() {
    this.renderData.length = 0;
    this.renderMonth = 'January';
  },
  methods: {
    ionInfinite(ev: InfiniteScrollCustomEvent) {
      switch (this.renderMonth) {
        case 'January':
          this.generateItems('February');
          break;
        case 'February':
          this.generateItems('March');
          break;
        case 'March':
          this.generateItems('April');
          break;
        case 'April':
          this.generateItems('May');
          break;
        case 'May':
          this.generateItems('June');
          break;
        case 'June':
          this.generateItems('July');
          break;
        case 'July':
          this.generateItems('August');
          break;
        case 'August':
          this.generateItems('September');
          break;
        case 'September':
          this.generateItems('October');
          break;
        case 'October':
          this.generateItems('November');
          break;
        case 'November':
          this.generateItems('December');
          break;
      }

      ev.target.complete();
    },
    generateItems(month: string) {
      this.renderMonth = month;

      HolidayData.holidays.forEach((h) => {
        if (
          h.name == this.renderMonth ||
          h.date.toString().includes(this.renderMonth)
        ) {
          this.renderData.push(h);
        }
      });
    },
  },
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
    NotifyButton,
    ShareButton,
    IonInfiniteScroll,
    IonInfiniteScrollContent,
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
