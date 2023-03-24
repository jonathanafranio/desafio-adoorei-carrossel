<template>
    <div class="loading">
        <div class="loading__loader"></div>
        <span class="loading__text">Carregando os produtos...</span>
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style scoped>
.loading {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 70vh;
}
.loading__loader {
    border: 5px solid #ccc;
    border-radius: 50%;
    border-top: 5px solid #e7d84b;
    width: 40px;
    height: 40px;
    -webkit-animation: spin 2s linear infinite; /* Safari */
    animation: spin 2s linear infinite;
}
.loading__text {
    margin-top: 10px;
    font-size: 1.6rem;
    color: #e7d84b;
}
@-webkit-keyframes spin {
    0% { -webkit-transform: rotate(0deg); }
    100% { -webkit-transform: rotate(360deg); }
}
  
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}
</style>