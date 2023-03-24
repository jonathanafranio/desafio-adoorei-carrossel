<template>
    <article 
        class="card" 
        :class="classCard">
        <figure v-if="image" class="card__fig">
            <img :src="image" :alt="title">
        </figure>
        <h2 class="card__title">{{ title }}</h2>
        <span class="card__price">{{ this.convert_real_coin(price) }}</span>
        <p class="card__txt">{{ this.description }}</p>
    </article>
</template>

<script>
    export default {
        props: {
            classCard: String,
            image: String,
            title: String,
            price: {
                type: Number,
                default(){ return 0 }
            },
            description: String,

        },
        methods: {
            convert_real_coin(num_value) {
                if(!num_value) return;
            
                return num_value.toLocaleString("pt-br", {
                    style: "currency",
                    currency: "BRL",
                });
            }
        }
    }
</script>
<style scoped>
.card {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 15px;
    background-color: #fff;
    border-radius: 10px;
    color: #000;
    
}
.card__fig {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 300px;
}
.card__title {
    font-size: 2.4rem;
    margin: 10px 0;
    color: #000;
}
.card__price {
    margin-top: 10px auto;
    font-size: 1.8rem;
    font-weight: 700;
    color: #360745;
}
.card__txt {
    margin-top: 10px;
    color: #444;
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