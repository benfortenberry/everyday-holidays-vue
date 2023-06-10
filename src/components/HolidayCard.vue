<template>
  <ion-card v-for="h in holidayData" v-bind:key="h.keyword">
    <img :src="`assets/imgs/${h.keyword}.jpg`" />

    <ion-card-header text-wrap>
      <ion-card-title>{{ h.name }}</ion-card-title>
    </ion-card-header>

    <ion-card-content class="white">
      {{ h.blurb }}
    </ion-card-content>

    <share-button :holiday="h" />
  </ion-card>
</template>

<script lang="ts">
import {
  IonCard,
  IonCardHeader,
  IonCardContent,
  IonCardTitle,
} from '@ionic/vue';
import { defineComponent } from 'vue';
import ShareButton from '../components/ShareButton.vue';

export default defineComponent({
  name: 'HolidayCard',
  data() {
    return {
      date: '',
      homeData: [{}],
    };
  },
  props: {
    holidayData: {},
  },
  methods: {},
  components: {
    IonCard,
    IonCardHeader,
    IonCardContent,
    IonCardTitle,
    ShareButton,
  },
});
</script>

<style scoped>
.white {
  color: white;
}
</style>
