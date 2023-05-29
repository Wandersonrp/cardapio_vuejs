<template>
  <div id="main-container">
    <Alert v-if="openAlert" :variant="typeAlert">
        <template v-slot:text-alert>
            {{ this.message }}
        </template>
    </Alert>
    <div>
        <form id="burguer-form">
            <div class="input-container">
                <label for="name">Nome do Cliente: </label>
                <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome">
            </div>
            <div class="input-container">
                <label for="bread">Escolha o pão:</label>
                <select name="bread" id="bread" v-model="bread">
                    <option value="">Selecione o pão</option>
                    <option :value="item.tipo" v-for="item in breads" :key="item.id">{{ item.tipo }}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="meat">Escolha o carne:</label>
                <select name="meat" id="meat" v-model="meat">
                    <option value="">Selecione a carne</option>                    
                    <option :value="item.tipo" v-for="item in meats" :key="item.id">{{ item.tipo }}</option>
                </select>
            </div>
            <div id="optionals-container" class="input-container">
                <label id="optionals-title" for="optionals">Selecione os opcionais:</label>
                <div class="checkbox-container" v-for="item in optionalsData" :key="item.id">
                    <input type="checkbox" name="optionals" v-model="optionals" :value="item.tipo">
                    <span>{{ item.tipo }}</span>                    
                </div>                                
            </div>
            <div class="input-container">
                <input type="submit" :class="submitBtn" value="Criar Burguer" @click="createBurguer">
            </div>
        </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Alert from '../components/Alert.vue';

export default {
    name: "Burguer-Form",
    data() {
        return {
            breads: null,
            meats: null,
            optionalsData: null,
            name: null,
            bread: null,
            meat: null,
            optionals: [],
            status: "Solicitado",
            message: null,
            submitBtn: "submit-btn",
            openAlert: false,
            typeAlert: ""
        }
    },
    methods: {
        async getIngredients() {
            try {                
                const response = await axios.get('http://localhost:3000/ingredientes');
                console.log(response);
                if (response.data != undefined) {
                    this.breads = response.data.paes;
                    this.meats = response.data.carnes;
                    this.optionalsData = response.data.opcionais;
                }
            } catch (err) {
                console.log(err);
            }
        },
        async createBurguer(e) {
            try {
                e.preventDefault();
                const data = {
                    nome: this.name,
                    carne: this.meat,
                    pao: this.bread,
                    opcionais: Array.from(this.optionals),
                    status: "Solicitado"
                }                

                const req = await axios.post('http://localhost:3000/burgers', data);
                if (req.status === 201) {
                    this.typeAlert = "success";
                    this.message = `Pedido Nº ${req.data.id} criado com sucesso`
                    this.openAlert = true;
                } else {
                    this.typeAlert = "danger";
                    this.message = "Erro ao criar pedido.";
                    this.openAlert = true;
                }

                this.name = "";
                this.meat = "";
                this.bread = "";
                this.optionals = [];
                setTimeout(() => this.openAlert= false, 3000);
                
            } catch (err) {
                console.log(err);
            }
        }
    },
    mounted() {
        this.getIngredients();
    },
    components: {
        Alert
    }
}
</script>

<style scoped>
#main-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

#burguer-form {
    max-width: 400px;
    /* margin: 0 auto; */
    padding-left: 50px;
}

.input-container {
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label {
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #f7a541;
}

input, select {
    padding: 5px 10px;
    width: 300px;
}

#optionals-container {
    flex-direction: row;
    flex-wrap: wrap;
}

#optionals-title {
    width: 100%;
}

.checkbox-container {
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
    width: auto;
}

.checkbox-container span {
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn {
    background-color: #ed303c;
    color: #fff;
    font-weight: bold;
    border: 2px solid #ed303c;
    padding: 10px;
    font-size: 16px;
    /* margin: 0 auto; */
    cursor: pointer;
    transition: .5s;
}

.submit-btn:hover {
    background-color: transparent;
    border: 2px solid #f7a541;
    color: #f7a541;
}
</style>