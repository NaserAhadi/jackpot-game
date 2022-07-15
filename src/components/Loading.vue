<template>
    <div class="loading">
    </div>
</template>

<script>
    export default {
        name: 'Loading'
    }
</script>

<style scoped>
.loading{
    display: flex;
    justify-content: center;

}

.loading::after{
    content: "";
    width: 50px;
    height: 50px;
    border: 10px solid #ddd;
    border-top-color: #43CBC0;
    border-bottom-color: #43CBC0;
    border-radius: 50%;
    animation: loading 1s linear infinite;
}

@keyframes loading {
    to {
        transform: rotate(1turn)
    }
}
</style>