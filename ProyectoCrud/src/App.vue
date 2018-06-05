<template>
  <div id="app">
    <nav class="navbar navbar-dark bg-primary">
      <a href="/" class="navbar-brand"> MG </a>
    </nav>

    <div class="container">
      <div class="row mt-5">
        <div class="col-sm-4">
          <div class="card">
            <div class="card-header">
                Agregar
            </div>
            <div class="card-body">
              <form @submit.prevent="addwebsite">
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newWebsite.name" placeholder="Nombre">              
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newWebsite.autor" placeholder="Autor">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newWebsite.url" placeholder="Direccion">
                </div>
                <button type="submit" class="btn btn-primary">Guardar</button>
              </form>
            </div>
          </div>
        </div>
        <div class="col-sm-8 text-center">
             <img src="./assets/logo.png">
             <div class="card">
             <div class="card-header">
               <H3>Websites List</H3>
             </div>
             <div class="card-body">
               <table class="table table-striped table-bordered">
                 <thead>
                   <th>Nombre</th>
                   <th>Autor</th>
                   <th>Operaciones</th>
                 </thead>
                 <tbody>
                    <tr v-for="w in websites">
                      <td>
                        <a v-bind:href="w.url" target="_blank">{{w.name}}</a>
                      </td>
                      <td>
                        {{w.autor}} 
                      </td>
                      <td>
                        <button class="btn btn-danger" @click="deletewebsite(w)" >Eliminar</button>
                      </td>
                    </tr>
                 </tbody>
               </table>
             </div>
             </div>
        </div>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
import Firebase from 'firebase';
import config from './config.js';
let app = Firebase.initializeApp(config);
let db = app.database();
let websiteref = db.ref('websites');
export default {
  name: 'App',
  firebase:{
    websites: websiteref
  },
  data() {
    return{
      newWebsite: {
        name: '',
        autor:'',
        url:''
      }
    }
  },
  methods: {
    addwebsite(){
     websiteref.push(this.newWebsite);
     this.newWebsite.name = '';
     this.newWebsite.autor = '';
     this.newWebsite.url = '';
    },
    deletewebsite(websites){
      websiteref.child(websites['.key']).remove();
    }
  }
}
</script>

<style>
</style>
