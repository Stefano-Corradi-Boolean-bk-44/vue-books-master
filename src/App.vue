<template>
  <div class="main-wrapper">
    <div class="container">
      <h1 class="text-center m-5">Ricerca libri</h1>
      
      <ListBooks />
    </div>
    
  </div>
</template>

<script>

import ListBooks from './components/ListBooks'


export default {
  name: 'App',
  components: {
    ListBooks,
  }
}
</script>

<style lang="scss">

@import './assets/style/vars.scss';
@import './assets/style/generals.scss';

.main-wrapper{
  min-height: 100vh;
  background-image: linear-gradient(to bottom,
    lighten($primary-color, 70%),
    lighten($primary-color, 20%)
  
  );

}


</style>
