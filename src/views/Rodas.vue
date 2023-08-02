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
        <ion-card v-for="noticia in noticias" style="margin-top: 20px; margin-bottom: 20px;">
          <img style="width: 120%; height: 200px" alt="Noticia" :src="noticia.imagen" />
          <ion-card-header>
            <ion-card-title>{{noticia.titulo}}</ion-card-title>
            <ion-card-subtitle>{{ noticia.fecha }}</ion-card-subtitle>
          </ion-card-header>
          <ion-card-content>
            {{ noticia.texto }}
          </ion-card-content>
        </ion-card>
        <div :hidden="textos.length !== 0" class="contenedor">
          <ion-spinner class="hijo" name="dots"></ion-spinner>
        </div>        
      </div>       
    </ion-content>
  </ion-page>
</template>

<script setup>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonCard, IonCardHeader, IonCardContent, IonCardTitle, onIonViewDidEnter, IonItem, IonSpinner } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { CapacitorHttp } from '@capacitor/core';
import * as cheerio from 'cheerio';
import { ref } from 'vue';

let titulos = ref([]);
let textos = ref([]);
let fechas = ref([]);
let imagenes = ref([]);
let noticias = ref([]);


onIonViewDidEnter(async () => {
  doGet()
})

const doGet = async() => {
  const options = {
    url: 'https://rodas.gob.cu',
  };

  const response = await CapacitorHttp.get(options);
  try{
    const $ = cheerio.load(response.data);
    const elTitulos = ".nspHeader";
    const elTextos = ".nspText";  
    const elFechas = ".nspInfo";
    const elImagenes = ".nspImage";
      
    $(elTitulos).each(function () {      
      const titulo = $(this).find("a").text();
      titulos.value.push(titulo);      
    });

    $(elTextos).each(function () {      
      const texto = $(this).text();
      textos.value.push(texto);      
    });

    $(elFechas).each(function () {      
      const fecha = $(this).text();
      fechas.value.push(fecha);      
    });

    $(elImagenes).each(function () {      
      const imagen = $(this).prop("src");
      imagenes.value.push(imagen);      
    });

    for(let x = 0; x < titulos.value.length; x++){
      noticias.value.push({
        titulo: titulos.value[x],
        texto: textos.value[x],
        fecha: fechas.value[x],
        imagen: imagenes.value[x],
      })
    }

  }catch(e){
    alert('Exite problemas con la conexión');
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