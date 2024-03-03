<template>
    <div 
        class="main">
        <div class="main__block">
            <h2 
                class="main__title"
            >{{ selectedProjectSpace ? selectedProjectSpace.name : 'Выберите проект'}}</h2>
            <button 
                class="main__btn"
                @click="$emit('remove', selectedProjectSpace)"
            >&#10006;</button>
        </div>
         
        <slot></slot> 
    </div>

</template>

<script>

export default {

    props: {
        selectedProjectSpace: {
            type: Object,
            default: null
        } 
    },
}
</script>

<style>
.main {
    position: relative;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    display: flex;
    flex-direction: column;
    align-items: start;
    min-height: 100vh;
    background: linear-gradient(to right, #3498db, #c0392b);
    padding: 0 10px 10px 30px;
    overflow: scroll;
    
}

.main__block {
    position: fixed;
    right: 0;
    bottom: 90%;
    left: 0;
    top: 80px;
    text-align: center;
    display: flex;
    align-items: center;
    
}
.main__title {
    position: fixed;
    right: 0;
    bottom: 90%;
    left: 50%;
    top: 70px;
    transform: translate(-50%, 0);
    margin: auto;
    font-size: 20px; 
}
.main__btn {
    position: fixed;
    right: 38px;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 20px;
}


</style>