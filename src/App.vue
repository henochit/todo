<template>
  <div id="app">
   <h4 class="bg-primary text-white text-center p-2">
      List de taches de {{nom}}
   </h4>
   <div class="container-fluid p-4">
    <div class="row" v-if="tacheFiltres.length == 0">
      <div class="col text-center">
        <b> Youpii Rien a faire </b>
      </div>
    </div>

    <template v-else>
      <div class="row">
        <div class="col font-weight-bold">Tache</div>
        <div class="col-2 font-weight-bold">Execute</div>
      </div>
      <div class="row" v-for="t in tacheFiltres" v-bind:key="t.action">
        <div class="col">{{t.action}}</div>
        <div class="col-2 test-center">
          <input type="checkbox" v-model="t.execute" class="form-check-input" />
        </div>
      </div>
    </template>

    <div class="row py-2">
      <div class="col">
        <input v-model="newTache" class="form-control" />
      </div>
      <div class="col-2">
        <button class="btn btn-primary" v-on:click="ajouterTache"> Add </button>
      </div>
    </div>

    <div class='row bg-secondary py-2 mt-2 text-white'>
      <div class="col text-center">
        <input type="checkbox" v-model="cacherExecute" class="form-check-input" />
        <label class="form-check-label font-weight-bold">
          Cacher les taches executees
        </label>
      </div>
      <div class="col text-center">
        <button class="btn btn-sm btn-warning" v-on:click="supprimerExecute">
          Supprimer Taches executes
        </button>
      </div>
    </div>
   </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  data(){
    return {
      nom: "Henochit",
      taches: [],
      cacherExecute: true,
      newTache: ""
    }
  },
    computed: {
      tacheFiltres(){
        return this.cacherExecute ?
          this.taches.filter(t => !t.execute) : this.taches
      }
    },
    methods: {
      ajouterTache(){
        this.taches.push({
          action: this.newTache,
          done: false
        });

        this.enregistreData();
        this.newTache = "";
      },
      enregistreData(){
        localStorage.setItem("taches", JSON.stringify(this.taches));
      },
      supprimerExecute(){
        this.taches = this.taches.filter(t => !t.execute);
        this.enregistreData();
      }
    },
      created(){
        let data = localStorage.getItem("taches");
        if (data != null) {
          this.taches = JSON.parse(data);
        }
      }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
