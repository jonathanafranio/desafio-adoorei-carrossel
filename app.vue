<template>
    <div>
        <h1 class="title-page">Carrossel - Desafio Front End Empresa Adoorei</h1>
        <div class="loading" v-if="loading">Carregando os produtos...</div>
        <div class="carrocel" v-else>
            <button class="carrocel__btn" @click="prev" v-if="carrocel_active > 0">
                <svg width="10" height="16" viewBox="0 0 10 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M8.57587 15.9979C8.90667 15.9985 9.22725 15.9057 9.48197 15.7357C9.62533 15.6399 9.74383 15.5224 9.83069 15.3897C9.91755 15.257 9.97106 15.1118 9.98816 14.9625C10.0053 14.8131 9.9856 14.6625 9.93032 14.5193C9.87505 14.3761 9.78523 14.243 9.66602 14.1278L3.32332 8.01564L9.43949 1.89208C9.5571 1.77543 9.64492 1.64122 9.69792 1.49715C9.75091 1.35308 9.76803 1.202 9.7483 1.05259C9.72856 0.90317 9.67236 0.758369 9.58292 0.626506C9.49348 0.494642 9.37257 0.378315 9.22713 0.284212C9.08064 0.180401 8.9091 0.102097 8.72326 0.054216C8.53742 0.00633469 8.3413 -0.0100903 8.14721 0.00597111C7.95311 0.0220325 7.76524 0.0702339 7.59537 0.14755C7.4255 0.224867 7.27731 0.329628 7.16009 0.455262L0.321867 7.29723C0.113632 7.50127 -0.000205718 7.75721 -0.000205741 8.02134C-0.000205764 8.28547 0.113631 8.54141 0.321867 8.74545L7.40077 15.5874C7.5428 15.7254 7.72321 15.8345 7.92738 15.9058C8.13155 15.9772 8.35372 16.0087 8.57587 15.9979Z" fill="currentColor"/>
                </svg>
            </button>

            <div class="carrocel__container">
                <article 
                    class="carrocel__item"
                    v-for="(prod, i) in products" 
                    :key="prod.id"
                    :class="classCard"
                    v-show="carrocel_active === i"
                >   
                    <figure v-if="prod.image" class="carrocel__fig">
                        <img :src="prod.image" :alt="prod.title">
                    </figure>

                    <h2 class="carrocel__title">{{ prod.title }} - {{ i }}</h2>
                    <span class="carrocel__price">{{ this.convert_real_coin(prod.price) }}</span>

                    <p class="carrocel__txt">{{ prod.description }}</p>
                </article>
            </div>

            <button class="carrocel__btn -next" @click="next" v-if="show_next">
                <svg width="10" height="16" viewBox="0 0 10 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M8.57587 15.9979C8.90667 15.9985 9.22725 15.9057 9.48197 15.7357C9.62533 15.6399 9.74383 15.5224 9.83069 15.3897C9.91755 15.257 9.97106 15.1118 9.98816 14.9625C10.0053 14.8131 9.9856 14.6625 9.93032 14.5193C9.87505 14.3761 9.78523 14.243 9.66602 14.1278L3.32332 8.01564L9.43949 1.89208C9.5571 1.77543 9.64492 1.64122 9.69792 1.49715C9.75091 1.35308 9.76803 1.202 9.7483 1.05259C9.72856 0.90317 9.67236 0.758369 9.58292 0.626506C9.49348 0.494642 9.37257 0.378315 9.22713 0.284212C9.08064 0.180401 8.9091 0.102097 8.72326 0.054216C8.53742 0.00633469 8.3413 -0.0100903 8.14721 0.00597111C7.95311 0.0220325 7.76524 0.0702339 7.59537 0.14755C7.4255 0.224867 7.27731 0.329628 7.16009 0.455262L0.321867 7.29723C0.113632 7.50127 -0.000205718 7.75721 -0.000205741 8.02134C-0.000205764 8.28547 0.113631 8.54141 0.321867 8.74545L7.40077 15.5874C7.5428 15.7254 7.72321 15.8345 7.92738 15.9058C8.13155 15.9772 8.35372 16.0087 8.57587 15.9979Z" fill="currentColor"/>
                </svg>
            </button>
        </div>
    </div>
</template>
<script>

export default {
    data() {
        return {
            loading: true,
            products: [],
            carrocel_active: 0,
            classCard: ''
        }
    },
    methods: {
        async request_api(url) {
            if(!url) return;

            console.log('chegou até aqui na promisse');
            await fetch(url)
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
    margin-top: 10px;
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
.carrocel__item {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 15px;
    background-color: #fff;
    border-radius: 10px;
    color: #000;
    
}
.carrocel__fig {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 300px;
}
.carrocel__title {
    font-size: 2.4rem;
    margin: 10px 0;
    color: #000;
}
.carrocel__price {
    margin-top: 10px auto;
    font-size: 1.8rem;
    font-weight: 700;
    color: #360745;
}
.carrocel__txt {
    margin-top: 10px;
    color: #444;
}
.carrocel__btn {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 40px;
    height: 40px;
    background-color: transparent;
    color: #e7d84b;
    border: none;
    cursor: pointer;
}
.carrocel__btn.-next {
    transform: rotate(180deg);
}
.carrocel__btn svg {
    transition: transform 0.7s ease-in-out;
    will-change: transform;
}
.carrocel__btn:hover svg {
    transform: scale(1.5);
}
.-fadeInRight {
    animation-name: fadeInRight;
    animation-iteration-count: 1;
    animation-duration: .3s;
}

.-fadeInLeft {
    animation-name: fadeInLeft;
    animation-iteration-count: 1;
    animation-duration: .3s;
}

@keyframes fadeInLeft {
    from {
        opacity: 0;
        transform: translate3d(-100%, 0, 0);
    }
  
    to {
        opacity: 1;
        -webkit-transform: translate3d(0, 0, 0);
        transform: translate3d(0, 0, 0);
    }
}

@keyframes fadeInRight {
    from {
        opacity: 0;
        transform: translate3d(100%, 0, 0);
    }
  
    to {
        opacity: 1;
        transform: translate3d(0, 0, 0);
    }
}
</style>