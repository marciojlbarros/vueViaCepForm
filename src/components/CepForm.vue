<template>
  <div class="container col-md-5">
    <h3 class="text-center mt-3">Vue Js e ViaCep API</h3>
    <p class="text-center mb-5">
      Criando um formulário reativo com Vue Js e ViaCep API
    </p>
    <form class="row g-3">
      <div class="form-group">
        <label for="">CEP</label>
        <input
          v-model="cep"
          placeholder="Digite seu cep"
          type="text"
          maxlength="8"
          class="form-control"
        />
      </div>
      
      <div v-if="response !== null" class="form-group">
        <div class="form-group" v-for="(value, index) in response" :key="index">
          <label for="">{{ index.toUpperCase() }}</label>
          <input
            class="form-control"
            :placeholder="index"
            v-model="response[index]"
            type="text"
          />
        </div>
      </div>

      <div class="col-12 text-center">
        <button class="btn btn-primary" type="reset">Limpar campos</button>
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CepForm",
  data() {
    return {
      cep: "",
      response: null,
      baseUrl: "https://viacep.com.br/ws/",
    };
  },
  methods: {
    getCep() {
      const url = `${this.baseUrl}${this.cep}/json/`;
      axios
        .get(url)
        .then((resp) => {
          const data = resp.data;
          if (!data.erro) {
            this.response = data;
          } else {
            alert("CEP não encontrado");
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  watch: {
    cep: function (novoCep) {
      if (novoCep.length === 8) this.getCep();
      else this.response = null;
    },
  },
};
</script>

<style scoped>
</style>