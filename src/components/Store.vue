<template>

    <div class="container">

        <div v-if="showNegativeAlert" class="alert alert-danger d-flex justify-content-between align-items-center"
            role="alert">
            You can't buy negative items.
            <button type="button" class="btn-close" @click="closeAlert"></button>
        </div>

        <div v-if="showExcessAlert" class="alert alert-danger d-flex justify-content-between align-items-center"
            role="alert">
            We don't have any more of this item.
            <button type="button" class="btn-close" @click="closeAlert"></button>
        </div>

        <div class="toast-container position-fixed top-0 start-50 translate-middle-x p-3">
            <div id="liveToast" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
                <div class="toast-header">
                    <img src="..." class="rounded me-2" alt="...">
                    <strong class="me-auto">Bootstrap</strong>
                    <small>11 mins ago</small>
                    <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
                </div>
                <!-- <div v-for="message in messages" class="toast-body">
                    {{ message }}
                </div> -->
                <div class="toast-body">
                    {{ message }}
                </div>
            </div>
        </div>



        <div class="row">
            <div v-for="item in items" class="col-md-3">
                <div class="card mb-4">
                    <img :src="item.sprites.default" class="card-img-top" alt="item">

                    <div class="card-body text-center">
                        <h5 class="card-title text-center">{{ item.name }}</h5>

                        <button class="btn mx-3" @click="deleteItem(item)">
                            <i class="bi bi-dash-lg"></i>
                        </button>

                        <label><strong>{{ item.quantityToBuy }}</strong></label>
                        <span v-if="item.quantity == 0"
                            class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger font-size-large">
                            {{ item.quantity }}
                        </span>

                        <span v-else
                            class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-success font-size-large">
                            {{ item.quantity }}
                        </span>

                        <button class="btn mx-3" @click="addItem(item)">
                            <i class="bi bi-plus-lg"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
        <div class="text-center m-3">
            <button class="btn btn-primary" @click="addToInventory()">
                BUY
            </button>
        </div>
    </div>

</template>
<script>
import axios from 'axios';
import * as bootstrap from 'bootstrap';

export default {
    data() {
        return {
            items: [],
            inventory: [],
            cart: [],
            showNegativeAlert: false,
            showExcessAlert: false,
            message: "hola",
            messages: {
                negativeArticle: "You can't buy negative items.",
                excessArticle: "We don't have any more of this item."
            }
        }
    },
    methods: {
        addItem(item) {
            // si el item ya está en el carrito -> True
            const existingItem = this.cart.find(cartItem => cartItem.name == item.name);

            if (item.quantity <= 0) {
                this.showExcessAlert = true;
            } else {
                if (existingItem) {
                    // si el item ya está en el carrito, solo modifico la cantidad
                    item.quantityToBuy += 1;
                    item.quantity -= 1;
                } else {
                    // si no está, push al array carrito
                    item.quantityToBuy += 1;
                    item.quantity -= 1;
                    this.cart.push(item);
                }
            }
            console.log(this.cart)
        },
        deleteItem(item) {
            const index = this.cart.findIndex(cartItem => cartItem.name == item.name);

            if (item.quantityToBuy <= 0) {
                // this.showNegativeAlert = true;
                this.message = this.messages.negativeArticle;
                this.showToast();
            } else {
                item.quantityToBuy -= 1;
                item.quantity += 1;
                if (item.quantityToBuy == 0) {
                    this.cart.splice(index, 1);
                }
                console.log(this.cart);
            }
        },
        addToInventory() {
            this.cart.forEach(item => {
                // si el item ya está en el INVENTARIO -> True
                const existingItem = this.inventory.find(inventoryItem => inventoryItem.name == item.name);

                if (existingItem) {
                    existingItem.inventoryQuantity += item.quantityToBuy;
                } else {
                    item.inventoryQuantity += item.quantityToBuy;
                    this.inventory.push(item);
                }
            });

            this.items.forEach(item => {
                item.quantityToBuy = 0;
            });

            this.cart = [];
            console.log("INVENTARIO BELLOW")
            // console.log("CARRITO: " + this.cart)
            // console.log(this.items);
            console.log(this.inventory);
        },
        getItems() {
            const me = this
            axios
                .get('https://pokeapi.co/api/v2/item')
                .then(response => {
                    const items = response.data.results;
                    console.log(response.data.results)

                    items.forEach(item => {
                        axios
                            .get(item.url)
                            .then(response => {
                                const itemData = response.data;
                                itemData.quantity = 20; // cantidad para la tienda
                                itemData.quantityToBuy = 0; // cantidad para comprar
                                itemData.inventoryQuantity = 0; // cantidad en el inventario
                                me.items.push(itemData);
                                // console.log(itemData);
                            })
                            .catch(error => {
                                console.error('Error al obtener los datos del Pokémon:', error);
                            });
                    });
                })
                .catch(error => {
                    console.error('Error al obtener la lista de Pokémon:', error);
                });
        },
        closeAlert() {
            this.showNegativeAlert = false;
            this.showExcessAlert = false;
        },
        showToast() {
            const toastElement = document.getElementById('liveToast');
            const toast = new bootstrap.Toast(toastElement);
            toast.show();
        }
    },
    created() {
        this.getItems();
    },
}
</script>
<style>
.font-size-large {
    font-size: 15px;
}
</style>