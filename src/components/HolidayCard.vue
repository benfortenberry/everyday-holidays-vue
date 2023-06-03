<template>
    
     <ion-card v-for="h in homeData" v-bind:key="h.keyword">
                <img :src="`assets/imgs/${h.keyword}.jpg`" />

                <ion-card-header text-wrap>
                  <ion-card-title>{{ h.name }}</ion-card-title>
                </ion-card-header>

                <ion-card-content class="white">
                  {{ h.blurb }}
                </ion-card-content>
                <ion-button @click="doShare(h.name)" fill="clear"
                  ><ion-icon :icon="shareSocial"></ion-icon
                ></ion-button>
                <!-- <ion-button fill="clear">Add to Calendar</ion-button> -->
              </ion-card>
</template>

<script>
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
    
})
</script>
