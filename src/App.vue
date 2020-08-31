<template>
  <div id="app">    
    <header>
      <nav class="navbar navbar-light bd-dark" style="background-color: #1DA1F1;">
        <h1 class="navbar-brand" href="#">B-Movies</h1>
      </nav>
    </header> 

    <main class="container mt-2">
      <form @submit.prevent="pegarResultados">
        <fieldset class="form-group">
          <label for="termoBusca">Busca</label>
          <input v-model="termoBusca" type="text" class="form-control" id="termoBusca" placeholder="Digite um título.">
        </fieldset>
        <button type="submit" class="btn btn-primary">Pesquisar</button>
      </form>

      <section>
        <section class="row movies-area">
          <section class="mt-2 col-9 row" id="resultados">

            <div v-if="error" class="alert alert-danger col" role="alert" >
              <p>Filme Não Encontrado. Tente outra vez.</p>
            </div>

            <div class="card col-4" v-for="resultado in resultados" v-bind:key="resultado" >
              <img class="card-img-top" :src="resultado.Poster" :alt="resultado.Title">
              <div class="card-body">
                <h5 class="card-title">{{resultado.Title}}</h5>
                <p class="card-text">{{resultado.Year}}</p>
              </div>
            </div>

          </section>
        </section>
      </section>     
    </main> 
  </div>
</template>

<script>
const API_URL = 'http://www.omdbapi.com/?apikey=fdf2fd5c&s='

export default {
  name: 'App',
  
  data: () => ({
    error: '',
    termoBusca: '',
    resultados: [],
  }),

  methods: {
    async pegarResultados(){
      const url = `${API_URL}${this.termoBusca}`
      const resposta = await fetch(url)
      const dados = await resposta.json()

      if(dados.Error) {
        console.log('entrou if')
        this.resultados = [];
        this.error = dados.Error;
      } else{
        console.log('entrou else')
        this.resultados = dados.Search;
        this.error = '';
      }      
    },
  }    
}
</script>

<style>
.movies-area {
  justify-content: space-around;
  align-items: flex-start;  
}
</style>
