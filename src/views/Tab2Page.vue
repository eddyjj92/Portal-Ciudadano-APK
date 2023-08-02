<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <img style="width: 100%; height: 100%;" src="/img/header.png" alt="">
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 2</ion-title>
        </ion-toolbar>
      </ion-header>
      <div style="align-items: center; justify-content: center;height: 70%; margin-top: 5%;">
        <img style="width: 100%; height: 120%;" src="/img/gobElec.png" alt="">
      </div> 
    </ion-content>
  </ion-page>
</template>

<script setup>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, onIonViewDidEnter, menuController } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { Storage } from '@ionic/storage';
import {ref} from 'vue';
import { Browser } from '@capacitor/browser';

const store = new Storage();
store.create();



</script>
