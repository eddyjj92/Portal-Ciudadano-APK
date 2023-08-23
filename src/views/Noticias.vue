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
          <ion-title size="large">Rodas</ion-title>
        </ion-toolbar>
      </ion-header>
      <div style="height: 60%; margin-top: 5%; margin-bottom: 5%;">
        <h5 class="ion-text-center" style="font-weight: bold; font-style: italic">Noticias Recientes
          <span v-if="props.municipio == 'aguadadepasajeros'" class="ion-text-capitalize">Aguada</span>
          <span v-else class="ion-text-capitalize">{{ props.municipio }}</span>
        </h5>
        <ion-card v-for="noticia in noticias" style="margin-top: 20px; margin-bottom: 20px;">
          <img style="width: 120%; height: 200px" alt="Noticia" :src="noticia.imagen" />
          <ion-card-header>
            <ion-card-title>{{noticia.titulo}}</ion-card-title>
            <ion-card-subtitle>{{ noticia.fecha }}</ion-card-subtitle>
          </ion-card-header>
          <ion-card-content>
            {{ noticia.noticia }}
          </ion-card-content>
        </ion-card>
        <div :hidden="textos.length !== 0 || error" class="contenedor">
          <ion-spinner class="hijo" name="dots"></ion-spinner>
        </div>
        <div :hidden="!(error && !fetching)" class="contenedor">
            <ion-icon class="hijo" @click="doGet()" slot="icon-only" :icon="refreshCircleOutline" size="large" color="primary"></ion-icon>
        </div>
      </div>
      <ion-alert
          :is-open="isOpen"
          header="Error de Conexión"
          message="No se ha podido establecer conexión con el servidor"
          :buttons="alertButtons"
          @didDismiss="setOpen(false)"
      ></ion-alert>
    </ion-content>
  </ion-page>
</template>

<script setup>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonCard, IonCardHeader, IonCardContent, IonCardTitle, onIonViewDidEnter, IonItem, IonSpinner } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { CapacitorHttp } from '@capacitor/core';
import * as cheerio from 'cheerio';
import { ref } from 'vue';
import { IonAlert, IonButton, IonIcon } from '@ionic/vue';
import { refreshCircleOutline } from 'ionicons/icons';


const props = defineProps({
  municipio: String
})

const isOpen = ref(false);
const alertButtons = ['Aceptar'];

const setOpen = (state) => {
  isOpen.value = state;
};

let error = ref(false);
let fetching = ref(true);

let titulos = ref([]);
let textos = ref([]);
let fechas = ref([]);
let imagenes = ref([]);
let noticias = ref([]);


onIonViewDidEnter(async () => {
  doGet()
})

const doGet = async() => {
  error.value = false;
  fetching.value = true;

  const options = {
    url: `https://portalciudadanoapi.000webhostapp.com/noticias/${props.municipio}`,
  };

  try {
    let response = await CapacitorHttp.get(options)
    noticias.value = response.data;
  } catch(err) {
    error.value = true;
    setOpen(true);
  } finally {
    fetching.value = false;
  }


};
</script>
<style scoped>
  .contenedor {
    position: relative;
  }
  .hijo {
    width: 100px;
    height: 100px;
    /* centrar vertical y horizontalmente */
    position: absolute;
    margin-top: 50% !important;
    left: 50% !important;
    margin: -50px 0 0 -50px; /* aplicar a top y al margen izquierdo un valor negativo para completar el centrado del elemento hijo */
  }
</style>