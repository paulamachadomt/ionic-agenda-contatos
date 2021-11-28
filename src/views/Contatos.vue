<template>
  <ion-page>
    <ion-app>
      <ion-header translucent> 
        <ion-toolbar>
          <div class="pHeader">
            <ion-title class="title">Contatos</ion-title>            
            <ion-fab vertical="center" horizontal="end" slot="fixed">
              <ion-fab-button @click="voltarParaHome()" color="primary"
                ><span class="btnAdd">↩</span></ion-fab-button
              >
            </ion-fab>
          </div>
        </ion-toolbar>       
        <!-- <ion-toolbar>
          <div class="pHeader">
            <ion-title class="pMarginDown title">Contatos</ion-title>
            <ion-button @click="voltarParaHome()" class="pBtnAdd"
              >Voltar</ion-button
            >
          </div>
        </ion-toolbar> -->
      </ion-header>
      <ion-content fullscreen>
        <label class="form-label pLabelStyle" for="nome">Nome</label>
        <input
          id="nome"
          class="form-control pStyle"
          type="text"
          v-model="contatos.nome"
        />
        <label class="form-label pLabelStyle" for="tipo">Tipo de contato</label>
        <input
          id="tipo"
          class="form-control pStyle"
          type="text"
          v-model="contatos.tipo"
        />
        <label class="form-label pLabelStyle" for="telefone">Telefone</label>
        <input
          id="telefone"
          class="form-control pStyle"
          type="text"
          v-model="contatos.telefone"
        />
        <label class="form-label pLabelStyle" for="email">E-Mail</label>
        <input
          id="email"
          class="form-control pStyle"
          type="text"
          v-model="contatos.email"
        />
      </ion-content>
      <ion-button @click="btnSalvar" class="pBtnAdd">Salvar</ion-button>
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
  IonApp,
} from "@ionic/vue";
import { defineComponent } from "vue";

export default defineComponent({
  name: "Contatos",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonApp,
  },
  props: ['id', 'termoBusca'],
  data() {
    return {
      uri: "https://crud-contatos-backend.herokuapp.com/",
      contatos: {},
    };
  },
  methods: {
    loadContatoPorId: async function(id){
      return fetch(this.uri + "contato/"+id,
        {
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          method: "GET",
        }).then((data) => data.json()).catch((error) => console.log(error));
    },
    voltarParaHome:function(){
        window.location.href="/home"},
    btnSalvar: function(){
      if(this.id>=1){
        this.atualizarContatoPorId(this.id);
      } else {
        this.cadastrarContato(this.contatos);
      }
    },
    atualizarContatoPorId:function(id){
      fetch(this.uri + "contato/"+id+"/atualizar",
        {
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.contatos),
          method: "PUT",
        }).then(alert('Contato atualizado')).catch((error) => console.log(error));
        window.location.href="/home"
        
    },
    cadastrarContato:function(){
      fetch(this.uri + "contato/",
        {
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.contatos),
          method: "POST",
        }).then((data) => data.json()).then(alert('Contato cadastrado')).catch((error) => console.log(error));
        window.location.href="/home"

    },
    extractContatos: function (promise) {
      const extract = async () => {
        const ext = await promise;
        this.contatos = ext; console.log(ext); console.log(this.contatos);
      };
      extract();
    },

  },
  beforeMount(){
    if (!this.id==0){
      console.log(this.id)
      this.extractContatos(this.loadContatoPorId(this.id))
      console.log(this.contatos)
    }
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
  /* margin: 10px; */
  display: flex;
}
.pBtnAdd {
  margin-right: 3%;
}
.pNavItems:hover {
  cursor: pointer;
  background-color: #c0c0c0;
}
.pStyle {
  max-width: 90%;
  margin: 5%;
  margin-top: 1%;
  padding: 7%;
  font-size: 2.5;
}
.pLabelStyle {
  margin: 5% 0 0px 5%;
  font-weight: 700;
}
.title {
  font-size: 25px;
  margin: 20px 20px 15px 20px;
} 
.btnAdd {
  font-size: 25px;
}
</style>