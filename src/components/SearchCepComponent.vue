<template>
    <div><h1 v-text="title"></h1>
    <form @submit.prevent="onSubmit">
        <input type="text" placeholder="Informe o CEP:" v-model="cep">
        <button type="submit">Buscar CEP</button>
    </form>
    <div v-if="preloader">
        <img src="../assets/preloader.gif" alt="Carregando...">
    </div>
    <div v-if="error != ''">
        {{error}}
    </div>
    <div v-show="address.localidade">
        <p><b>Logradouro:</b> {{ address.logradouro}}</p>
        <p><b>Bairro:</b> {{ address.bairro}}</p>
        <p><b>Localidade:</b> {{ address.localidade}}</p>
        <p><b>Uf:</b> {{ address.uf}}</p>

    </div>
    </div>
</template>
<script>
export default {
    data (){
        return {
            title: 'Buscar CEP',
            cep: '',
            address: {},
            preloader: false,
            error: ''
        }
    },
    methods:{
        onSubmit(){

            this.reset()
            
            this.preloader = true
            this.$http.get('https://viacep.com.br/ws/'+this.cep+'/json/')
            .then( response => {
                this.address = response.body
            }, error =>  { 
                console.log(error)
                this.error = 'CEP informado não encontrado'
            })
            .finally(() => {
                this.preloader = false
            })
        },
        reset (){
            this.error = '',
            this.address = {}
        }
    }
}
</script>
<style scoped>

</style>