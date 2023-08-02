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
          <ion-title size="large">Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>
      <div style="align-items: center; justify-content: center;height: 14%; padding: 10px;">
        <div @click="navigate(`aguadadepasajeros`)" style="width: 17%;height: 100%; margin-left: 3%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/ag.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
        <div @click="navigate(`abreus`)" style="width: 17%;height: 100%; margin-left: 3%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/ab.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
        <div style="width: 17%;height: 100%; margin-left: 3%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <router-link to="rodas">
            <img style="width: 100%; height: 100%;" src="/img/ro.png" alt="">
          </router-link>          
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
        <div @click="navigate(`lajas`)" style="width: 17%;height: 100%; margin-left: 3%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/la.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
        <div @click="navigate(`cruces`)" style="width: 17%;height: 100%; margin-left: 3%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/cr.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
      </div> 
      <div style="align-items: center; justify-content: center;height: 70%;">
        <img style="width: 100%;height: 100%;" src="/img/body.png" alt="">
      </div>    
      <div style="height: 14%; padding: 10px;">
        <div @click="navigate(`cienfuegos`)" style="width: 17%;height: 100%; margin-left: 8%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/cfprov.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>    
        <div @click="navigate(`cienfueguero`)" style="width: 17%;height: 100%; margin-left: 8%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/cf.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
        <div @click="navigate(`palmira`)" style="width: 17%;height: 100%; margin-left: 8%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/pa.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>   
        <div @click="navigate(`cumanayagua`)" style="width: 17%;height: 100%; margin-left: 8%; border-radius: 10px; display: inline-block;" class="ion-activatable ripple-parent">
          <img style="width: 100%; height: 100%;" src="/img/cu.png" alt="">
          <ion-ripple-effect></ion-ripple-effect>
        </div>      
      </div> 
    </ion-content>
  </ion-page>
</template>

<script setup>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonModal, onIonViewDidEnter } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import {ref} from 'vue';
import { Storage } from '@ionic/storage';
import { Browser } from '@capacitor/browser';

const store = new Storage();
store.create();

const navigate = async(municipio) => {
  await Browser.open({ url: `https://${municipio}.gob.cu` });
}

</script>

<style scoped>
  .ripple-parent {
    position: relative;
    overflow: hidden;
  }
</style>
