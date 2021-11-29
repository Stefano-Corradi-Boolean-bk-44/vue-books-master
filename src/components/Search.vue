<template>
  <div class="row my-5">

    <div class="col-2">
      <p class="text-end pt-2">Ricerca per: </p>
    </div>
    <div class="col-2">
      <select v-model="type" class="form-select" >
        <option value="author">Autore</option>
        <option value="title">Titolo</option>
      </select>
    </div>
    <div class="col-6">
      <input
      v-model="toSearch"
      @keyup.enter="sendSerach"
       type="text" class="form-control" placeholder="Cosa devi cercare?">
    </div>
    <div class="col-1">
      <button @click="sendSerach" class="btn btn-info">Cerca</button>
    </div>
    <div class="col-1">
      <button @click="resetSearch" class="btn btn-warning">Reset</button>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Search',
  data(){
    return{
      toSearch: '',
      type:'',
    }
  },
  methods:{
    resetSearch(){
      this.toSearch = '',
      this.type = ''
      this.$emit('reset')
    },
    sendSerach(){
      if(this.toSearch.length === 0 || this.type.length === 0 ){
        alert('Attenzione!\rInserire entrambi i criteri di ricerca')
      }else{
        const serachData = {
          toSearch: this.toSearch,
          type: this.type
        }

        // per la ricerca invio come paramtro un oggetto che contiene i tipo e la stringa di ricerca
       this.$emit('search',serachData);
       
      }
      
    },
  }
}
</script>

<style>

</style>