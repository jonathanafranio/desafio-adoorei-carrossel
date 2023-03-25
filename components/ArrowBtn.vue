<template>
    <button 
        :class="next ? 'arrow-btn -next' : 'arrow-btn'" 
        @click="actionClick">
        <svg width="10" height="16" viewBox="0 0 10 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M8.57587 15.9979C8.90667 15.9985 9.22725 15.9057 9.48197 15.7357C9.62533 15.6399 9.74383 15.5224 9.83069 15.3897C9.91755 15.257 9.97106 15.1118 9.98816 14.9625C10.0053 14.8131 9.9856 14.6625 9.93032 14.5193C9.87505 14.3761 9.78523 14.243 9.66602 14.1278L3.32332 8.01564L9.43949 1.89208C9.5571 1.77543 9.64492 1.64122 9.69792 1.49715C9.75091 1.35308 9.76803 1.202 9.7483 1.05259C9.72856 0.90317 9.67236 0.758369 9.58292 0.626506C9.49348 0.494642 9.37257 0.378315 9.22713 0.284212C9.08064 0.180401 8.9091 0.102097 8.72326 0.054216C8.53742 0.00633469 8.3413 -0.0100903 8.14721 0.00597111C7.95311 0.0220325 7.76524 0.0702339 7.59537 0.14755C7.4255 0.224867 7.27731 0.329628 7.16009 0.455262L0.321867 7.29723C0.113632 7.50127 -0.000205718 7.75721 -0.000205741 8.02134C-0.000205764 8.28547 0.113631 8.54141 0.321867 8.74545L7.40077 15.5874C7.5428 15.7254 7.72321 15.8345 7.92738 15.9058C8.13155 15.9772 8.35372 16.0087 8.57587 15.9979Z" fill="currentColor"/>
        </svg>
    </button>
</template>

<script>
    export default {
        props: {
            next: {
                type: Boolean,
                default(){ return false }
            }
        },
        methods: {
            actionClick() {
                this.$emit('action');
            }
        }
    }
</script>

<style scoped>
.arrow-btn {
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
.arrow-btn.-next {
    transform: rotate(180deg);
}
.arrow-btn svg {
    transition: transform 0.7s ease-in-out;
    will-change: transform;
}
.arrow-btn:hover svg {
    transform: scale(1.5);
}
</style>