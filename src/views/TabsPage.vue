<template>
  <ion-menu menu-id="first-menu" content-id="main-content">
      <ion-header>
        <ion-toolbar color="tertiary">
          <ion-title>Menú Principal</ion-title>
        </ion-toolbar>
      </ion-header>
    <ion-content class="ion-padding">
      <ion-list>
        <ion-item>
          <ion-label><ion-button @click="closeFirstMenu" router-link="/tabs/tab1" size="large" fill="clear" expand="full">
            <ion-icon slot="start" :icon="home"></ion-icon>
            Inicio</ion-button>
          </ion-label>
        </ion-item>
        <ion-item>
          <ion-label><ion-button @click="closeFirstMenu" router-link="/tabs/tab2" size="large" fill="clear" expand="full">
            <ion-icon slot="start" :icon="helpCircleOutline"></ion-icon>
            Ayuda</ion-button>
          </ion-label>
        </ion-item>
        <ion-item>
          <ion-label><ion-button @click="closeFirstMenu" router-link="/tabs/tab3" size="large" fill="clear" expand="full">
            <ion-icon slot="start" :icon="informationCircleOutline"></ion-icon>
            Acerca</ion-button>
          </ion-label>
        </ion-item>
        <ion-item>
          <ion-label><ion-button @click="presentAlert" size="large" fill="clear" expand="full">
            <ion-icon slot="start" :icon="exit"></ion-icon>
            Cerrar</ion-button>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-menu>
  <ion-page id="main-content">   
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar slot="bottom" style="height: 10%;">
        <ion-tab-button tab="tab1" @click="goBack">          
          <ion-label><img style="width: 90%;" src="/img/back.png" alt=""></ion-label>
        </ion-tab-button>
        <ion-tab-button tab="tab2" href="tab2">      
          <ion-label><img style="width: 90%;" src="/img/help.png" alt=""></ion-label>
        </ion-tab-button>
        <ion-tab-button tab="tab3" @click="presentAlert">
          <ion-label><img style="width: 90%;" src="/img/salir.png" alt=""></ion-label>
        </ion-tab-button>
        <ion-tab-button tab="tab4" @click="openFirstMenu()">
          <ion-label><img style="width: 90%;" src="/img/conf.png" alt=""></ion-label>
        </ion-tab-button>
        <ion-tab-button tab="tab5">
          <ion-label><img @click="goForward" style="width: 90%;" src="/img/forward.png" alt=""></ion-label>
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script setup lang="ts">
import { IonTabBar, IonTabButton, IonTabs, IonLabel, IonIcon, IonPage, IonRouterOutlet, IonAlert, alertController,onIonViewDidEnter, useIonRouter, IonMenu, IonHeader, IonButton, menuController, IonItem, IonRadio, IonRadioGroup, IonList } from '@ionic/vue';
import { ellipse, square, triangle, star, helpCircleOutline, home, informationCircleOutline, exit } from 'ionicons/icons';
import { ref } from 'vue';
import router from '../router';
import { App } from '@capacitor/app';

const ionRouter = useIonRouter();




onIonViewDidEnter(async () => {

});


const presentAlert = async () => {
  playNotifi();
  const alert = await alertController.create({
          header: 'Portal del Ciudadano',
          subHeader: 'Salir de la Aplicación',
          message: '¿Está seguro q desea salir?',
          buttons: alertButtons,          
        });     
  await alert.present(); 
}

App.addListener('backButton', ({canGoBack}) => {
  canGoBack ? window.history.back() : presentAlert();
});

const handlerMessage = ref('');
const roleMessage = ref('');

const alertButtons = [
        {
          text: 'Cancelar',
          role: 'cancel',
          handler: () => {
            handlerMessage.value = 'Alert canceled';
          },
        },
        {
          text: 'Salir',
          role: 'confirm',
          handler: () => {
            handlerMessage.value = 'Alert confirmed';
            App.exitApp()
          },
        },
];

const goBack = async() => { 
  if (!ionRouter.canGoBack()) {
    presentAlert()
  }else{
    ionRouter.back()
  }
}  

const goForward= async() => {   
  ionRouter.forward()
}  

const openFirstMenu = async () => {  
  // Open the menu by menu-id
  await menuController.enable(true, 'first-menu');
  await menuController.open('first-menu');
};

const closeFirstMenu = async () => {
  await menuController.close('first-menu');
}

const playNotifi = () => {
  const audio = new Audio('/sound/Notification-magic-light.mp3');
  audio.play();
}   

</script>
<style scoped>
  ion-menu::part(backdrop) {
    background-color: rgba(150, 152, 252, 0.5);
  }

  ion-menu::part(container) {
    border-radius: 0 20px 20px 0;

    box-shadow: 4px 0px 16px rgba(35, 13, 156, 0.548);
  }
</style>
