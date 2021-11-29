<template>
  <div>

    <!-- 2 eventi diverersi per la ricerca e per il reset -->
    <Search 
      @search="performSearch"
      @reset="reset"
     />

     <!-- stampo solo all'inizio, quando resetto e se il risultato è vuoto -->
    <div v-if="emptyData">
      <!-- messageEmpty è diverso se la ricerca è vuota o se siamo all'inizio/reset -->
      <!-- v-html serve per stampare data che contengono tag HTML -->
      <div class="text-center m-5" v-html="messageEmpty" ></div>
    </div>

    <!-- stampo solo quando il risultato della ricerca abbia popolato l'array books -->
    <div v-else>
      <div v-if="isLoaded">
        <h2>Chiave di ricerca: {{type}} - {{toSearch}}</h2>
          <table class="table table-striped table-hover my-5">
            <thead>
              <tr>
                <th scope="col">#</th>
                <th scope="col">Autore</th>
                <th scope="col">Titolo</th>
                <th scope="col">Anno di pubblicazione</th>
                <th scope="col">Anno ultima edizione</th>
                <th scope="col">ISBN ultima edizione</th>
              </tr>
            </thead>
            <tbody >
              <tr
                v-for="(book, index) in books"
              :key="book.key"
              >
                <th scope="row">{{index+1}}</th>
                <td>
                  <!-- ci possono essere più autori infatti il dato è un array -->
                  <p v-for="(author, key) in book.author_name" :key="key">{{author}}</p>
                </td>
                <td>{{book.title}}</td>
                <td>{{book.first_publish_year}}</td>
                <td>{{lastYear(book)}}</td>
                <td>{{isbn(book)}}</td>
              </tr>
            </tbody>
        </table>
      </div>
      <Loader v-else titleLoader="caricamento libri in corso..." />
    </div>
  </div>
</template>

<script>
import Search from './Search'
import Loader from './Loader'
import axios from 'axios';

export default {
  name: 'ListBooks',
  components:{
    Search,
    Loader
  },
  data(){
    return{
      books:[],
      apiUrl: 'http://openlibrary.org/search.json?',
      type: '',
      toSearch: '',
      isLoaded: false,
      messageEmpty: '<h3>Insierisci le chiavi di ricerca e premi invio</h3>',
      emptyData: true
    }
  },
  methods:{
    // richiamata dall' $emit @search
    performSearch(data){
      // valorizzo i miei data per la ricerca...
      this.type = data.type;
      this.toSearch = data.toSearch;
      //... e chiamo l'API
      this.getApi();
    },
    // richiamata dall'$emit @reset
    reset(){
      // ripristino lo satato di partenza
      this.messageEmpty = '<h3>Insierisci le chiavi di ricerca e premi invio</h3>';
      this.emptyData = true;
      this.type = ''
      this.toSearch = '';
      this.books = [];
    },
    getApi(){
      this.messageEmpty = '';
      this.emptyData = false;
      this.isLoaded = false;
      // la stringa di ricerca è frutto della concatenazione dei miei data
      axios.get(`${this.apiUrl}${this.type}=${this.toSearch}`)
      .then(r =>{
        this.books = r.data.docs;
        this.isLoaded = true;
        // se non trovo nulla stampo la pagina vuota e il messaggio di specifica della ricerca fallita
        if(this.books.length === 0){
          this.messageEmpty = `Nessun libro trovato per questa ricerca
          <br>
          tipo: <strong>${this.type}</strong> - chiave: <strong>${this.toSearch}</strong>`;
          this.emptyData = true;
        }
      })
      .catch( e => {
        console.log(e);
      })
    },
    // evito il possibile errore che book.isbn sia undefined
    isbn(book){
      if(book.isbn === undefined){
        return 'NN'
      }
      return book.isbn[book.isbn.length - 1]
    },
    // evito il possibile errore che book.publish_year sia undefined
    lastYear(book){
      if(book.publish_year === undefined){
        return 'NN'
      }
      return book.publish_year[book.publish_year.length - 1]
    }
  }
}
</script>

<style>

</style>