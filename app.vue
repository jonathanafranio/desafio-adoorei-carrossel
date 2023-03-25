<template>
    <div>
        <h1 class="title-page">Carrossel - Desafio Front End Empresa Adoorei</h1>
        <Preload class="loading" v-if="loading" />
        <div class="carrocel" v-else>
            <ArrowBtn 
                v-on:action="prev" 
                v-if="carrocel_active > 0" />
            <div class="carrocel__container">
                <template  
                    v-for="(prod, i) in products" 
                    :key="prod.id">
                    <CardCarrossel 
                        :classCard="classCard"
                        :image="prod.image" 
                        :title="prod.title" 
                        :price="prod.price" 
                        :description="prod.description"
                        v-show="carrocel_active === i"
                    />
                </template>
            </div>

            <ArrowBtn 
                v-on:action="next" 
                :next="true"
                v-if="show_next" />
        </div>
    </div>
</template>
<script>
import Preload from './components/Preload.vue';
import CardCarrossel from './components/CardCarrossel.vue';
import ArrowBtn from './components/ArrowBtn.vue';

export default {
    data() {
        return {
            loading: true,
            products: [],
            carrocel_active: 0,
            classCard: ''
        }
    },
    components: {
        Preload,
        CardCarrossel,
        ArrowBtn
    },
    methods: {
        async request_api(url) {
            if(!url) return;

            fetch(url)
                .then(r => r.json())
                .then(r => {
                    this.products = r;
                    this.loading = false;
                })
                .catch(e => {
                    this.loading = false;
                    console.error('Erro ao requistar os produtos. :(', e);
                })
        },
        convert_real_coin(num_value) {
            if(!num_value) return;
            
            return num_value.toLocaleString("pt-br", {
                style: "currency",
                currency: "BRL",
            });
        },
        next() {
            const total_carrocel = this.products.length-1;
            const carrocel_active = this.carrocel_active;

            if(carrocel_active < total_carrocel) {
                const old_state =  carrocel_active;
                this.carrocel_active = carrocel_active + 1;
                this.fade(old_state, this.carrocel_active)
            }
        },
        prev() {
            const carrocel_active = this.carrocel_active;

            if(carrocel_active > 0) {
                const old_state =  carrocel_active;
                this.carrocel_active = carrocel_active - 1;
                this.fade(old_state, this.carrocel_active)
            }

        },
        fade(oldStep, currentStep) {
            let classReturn = '';
            if(oldStep > currentStep) {
                classReturn = '-fadeInLeft';
            } else {
                classReturn = '-fadeInRight';
            }


            this.classCard = classReturn;
        },


    },
    computed: {
        show_next() {
            const last_index = this.products.length-1
            return this.carrocel_active < last_index ? true : false;
        }
    },
    created() {
        this.request_api('https://fakestoreapi.com/products');
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
html {
  font-size: 62.5%;
}
body {
    background: #360745;
    color: #fff;
    font-size: 1.6rem;
    font-family: Arial, Helvetica, sans-serif;
}
img {
    max-width: 100%;
    height: 100%;
}
.title-page {
    margin: 15px;
    font-size: 2.5rem;
    text-align: center;
}
.carrocel {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 10px;
    width: 100%;
}
.carrocel__container {
    display: block;
    width: calc(100% - 80px);
    max-width: 320px;
}



</style>