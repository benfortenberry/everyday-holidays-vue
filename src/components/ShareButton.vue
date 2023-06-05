<template>
  <ion-button @click="doShare(holiday.name, holiday.date)" fill="clear"
    ><ion-icon color="dark" :icon="shareSocial"></ion-icon
  ></ion-button>
</template>

<script lang="ts">
import { IonButton, IonIcon } from '@ionic/vue';
import { defineComponent } from 'vue';
import { shareSocial } from 'ionicons/icons';
import { Share } from '@capacitor/share';

export default defineComponent({
  name: 'HolidayCard',
  data() {
    return {
      shareSocial,
    };
  },
  props: {
    holiday: {},
  },
  methods: {
    async doShare(holidayName: any, date: any) {
      try {
        await Share.share({
          text: date + ' is ' + holidayName,

          dialogTitle: 'Share holiday',
        });
      } catch {
        console.log('error');
      }
    },
  },
  components: {
    IonButton,
    IonIcon,
  },
});
</script>

<style scoped>
.white {
  color: white;
}
</style>
