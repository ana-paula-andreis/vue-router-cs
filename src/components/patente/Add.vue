<template>
    <div class="submit-form">
      <div v-if="!submitted">       

        <div class="form-group">
            <label for="inputId">Id:</label>
            <input type="text" v-model="patente.id" class="form-control" id="inputId">
        </div>

        <div class="form-group">
            <label for="inputNome">Nome:</label>
            <input type="text" v-model="patente.nome" class="form-control" id="inputNome">
        </div>
        <div class="form-group">
            <label for="inputCor">Cor:</label>
            <input type="password" v-model="patente.cor" class="form-control" id="inputCor">
        </div>            
      </div>     
  
        <button @click="savepatente" class="btn btn-success">Salvar</button>
        <router-link to="/patentes" class="btn btn-success">Voltar</router-link>                

      </div>
  
      <div v-else>
        <h4>Dados enviados com sucesso !</h4>
        <button class="btn btn-success" @click="newpatente">Novo</button>
        <router-link to="/patentes" class="btn btn-success">Voltar</router-link>
      </div>
    </div>
  </template>

<script>

    import patenteDataService from '../../services/patenteDataService'
    import PatenteDataService from '../../services/PatenteDataService'

    export default {
        name: "addPatente",
        data(){
            return {
                patente: {indice: '', 
                                    nome: '', 
                                    cor: '',
                submitted: false
            }            
        },
        methods: {

            savePatente(){

                var jgd = jQuery.extend({}, this.patente);//clona o this.novo_patente e armazena na variavel patente. dessa forma alteracoes em this.novo_patente nao irao refletir em patente.

                if (jgd.nome.trim().length > 0 && jgd.cor.trim().length > 0 &&
                {
                
                    patenteDataService.create(jgd)
                    .then(response => {
                        
                        this.submitted = true;
                    })
                    .catch(e => {                        
                        alert("Erro ao tentar cadastrar. !!! " + e.message);

                    })

                }else{
                    alert('Formulário incompleto !!!');
                }

            },
            newpatente(){

                this.submitted = false;
                this.patente = {};
            },
            listPatentes(){

                PatenteDataService.list().then(response =>{

                console.log("Retorno do seviço PatenteDataService.list", response.status);

                for(let j of response.data){

                    this.patentes.push(j);
                }                  

                }).catch(response => {

                // error callback
                alert('Não conectou no serviço PatenteDataService.list');
                console.log(response);
                });               
            }

        },
        mounted() {                        
            this.listPatentes();
            
         }

    }

</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>