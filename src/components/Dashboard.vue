<template>
    <Alert v-if="openAlert" :variant="typeAlert">
        <template v-slot:text-alert>{{ message }}</template>
    </Alert>
  <div id="burguer-table">
    <div>
        <div id="burguer-table-heading">
            <div class="order-id">#</div>
            <div>Cliente:</div>
            <div>Pão:</div>
            <div>Carne:</div>
            <div>Opcionais:</div>
            <div>Ações:</div>
        </div>
        <div id="burguer-table-rows">
            <div class="burguer-table-row" v-for="item in orders" :key="item.id">
                <div class="order-number">{{ item.id }}</div>
                <div>{{ item.nome }}</div>
                <div>{{ item.pao }}</div>
                <div>{{ item.carne }}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in item.opcionais" :key="index">{{ opcional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="status" id="status" @change="updateStatus($event, item.id)">
                        <option value="">Selecione</option>
                        <option :value="s.tipo" v-for="s in status" :key="s.id" :selected="item.status === s.tipo">{{ s.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deleteOrder(item.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Alert from '../components/Alert.vue';

export default {
    name: "Dashboard-App",
    data() {
        return {
            orders: [],
            status: [],
            openAlert: false,
            message: "",
            typeAlert: ""
        }
    },
    components: {
        Alert
    },
    methods: {
        async getOrders() {
            try {
                const response = await axios.get("http://localhost:3000/burgers");
                if (response.data != undefined) {
                    this.orders = response.data;
                    const status = await this.getStatus();
                    this.status = status;
                }
            } catch (err) {
                console.log(err);
            }
        },
        async getStatus() {
            try {
                const response = await axios.get("http://localhost:3000/status");
                if (response.data != undefined) {
                    return response.data;
                }
            } catch (err) {
                console.log(err);
            }
        },
        async deleteOrder(id) {
            try {
                const response = await axios.delete("http://localhost:3000/burgers/" + id);
                if (response.status === 200) {
                    this.openAlert = true;
                    this.message = `Pedido Nº ${id} cancelado com sucesso`;
                    this.typeAlert = "success";
                    setTimeout(() => this.openAlert = false, 3000);
                }
                this.getOrders();
            } catch (err) {
                console.log(err);
            }
        },
        async updateStatus(event, id) {
            try {
                const option = event.target.value;
                await axios.patch(`http://localhost:3000/burgers/${id}`, { status: option });
            } catch (err) {
                console.log(err);
            }
        }
    },
    mounted() {
        this.getOrders();
    }
}
</script>

<style scoped>
#burguer-table {
    max-width: 1200px;
    min-height: 300px;
    margin: 0 auto;
}

#burguer-table-heading,
#burguer-table-rows,
.burguer-table-row {
    display: flex;
    flex-wrap: wrap;
}

#burguer-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

#burguer-table-heading div,
.burguer-table-row div {
    width: 19%;
}

.burguer-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #ccc;
}

#burguer-table-heading .order-id,
.burguer-table-row .order-number {
    width: 5%;
}

select {
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #ed303c;
    color: #fff;
    font-weight: bold;
    border: 2px solid #ed303c;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    background-color: transparent;
    color: #f7a541;
    border: 2px solid #f7a541;
}
</style>