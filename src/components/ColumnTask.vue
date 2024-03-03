<template>
    
        <div class="task" 
            draggable="true"
        >
            <h4>{{ task.title }}</h4>
            <p>{{ task.descr }}</p>
        </div>   
</template>

<script>

export default {
    props: {
       
        task: {
            type: Object,
            default: null
        },
        draggable: {
            type: Boolean
        }  
    },

    data() {
        return {
            
            
        }
    }, 
    methods: {
        
    }
}
</script>

<style>
.task {
    display: flex;
    flex-direction: column;
    width: 250px;
    border: 1px solid black;
    border-radius: 5px;
    background-color: #c2ddef;
    padding: 5px 10px;
    margin: 5px;
} 

.task h4, .task p {
    overflow-wrap: break-word
}
</style>