<template>
  <ion-page>
    <ion-app>
      <ion-header translucent>
        <ion-toolbar>
          <div class="pHeader">
            <ion-title class="title">Contatos</ion-title>            
            <ion-fab vertical="center" horizontal="end" slot="fixed">
              <ion-fab-button @click="criarNovoContato" color="danger"
                ><span class="btnAdd">+</span></ion-fab-button
              >
            </ion-fab>
          </div>
        </ion-toolbar>        
      </ion-header>
      <ion-content fullscreen>
        <ion-list>
          <ion-item
            v-for="contato in contatos"
            :key="contato.index"
            class="pNavItems"
          >
            <span @click="abrirContatoPorId(contato.id)" class="pSpanWidth">{{
              contato.nome
            }} </span>
            <span class="pNavItemsBtnRight"
              ><ion-button
                @click="deletarContatoPorId(contato.id)"
                color="danger"
                size="small"
                >X</ion-button
              ></span
            >
          </ion-item>
        </ion-list>
      </ion-content>
    </ion-app>
  </ion-page>
</template>

<script lang="js">

import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonItem,
  IonList,
  IonApp,
  IonFab,
  IonFabButton,
} from "@ionic/vue";

import { defineComponent } from "vue";

export default defineComponent({
  name: "Home",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonToolbar,
    IonItem,
    IonList,
    IonApp,
    IonFab,
    IonTitle,
    IonFabButton
  },
  props: ['termoBusca'],
  data() {
    return {
      uri: "https://crud-contatos-backend.herokuapp.com/",
      contatos: [],
      termoBuscaLive: ""
    };
  },
  methods: {
    loadAllContatos: async function(){
      return fetch(this.uri + "contato",
        {
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          method: "GET",
        }).then((data) => data.json()).catch((error) => console.log(error));
    },
    extractContatos: function (promise) {
      const extract = async () => {const ext = await promise; this.contatos = ext}
      extract();
    },
    abrirContatoPorId: async function(id){ this.$router.push({name:'Contatos', params: {id: id}});},
    deletarContatoPorId: async function(id){
      const resposta = window.confirm("Vai deletar mesmo?");
      if (resposta) {
        await fetch(this.uri + "contato/"+id,
        {
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          method: "DELETE",
        }); 
        window.location.reload();
      }      
    },
    criarNovoContato: async function(){this.$router.push({name:'Contatos', params: {id: 0}});},
  },
  beforeMount(){
      this.extractContatos(this.loadAllContatos());


  },
});

</script>
<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
.pHeader {
  display: flex;
}
.pBtnAdd {
  margin-right: 3%;
}

.pNavItems:hover {
  cursor: pointer;
  background-color: #c0c0c0;
}

.pNavItemsBtnRight {
  position: absolute;
  right: 5%;
}
.pSpanWidth{
  min-width: 80%;
}
.btnAdd {
  font-size: 25px;
}
.title {
  font-size: 25px;
  margin: 20px 20px 15px 20px;
} 
</style>
