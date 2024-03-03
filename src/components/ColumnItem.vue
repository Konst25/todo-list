<template>
    
        <div 
            class="column"
            v-for="(column, columnIndex) in selectedProjectSpace.columns"
            :key="columnIndex"
            @dragover.prevent
            @drop="onDrop($event, columnIndex)"
        >
            <div class="column__title">
                    <button class="column__title_btn" @click="openTaskForm(column)">&#9998;</button>
                    <h3>{{ column.title }}</h3>
                    <button class="column__title_btn" @click="$emit('remove', column)">&#10006;</button>
            </div>

                <column-task
                    v-if="selectedColumn"
                    v-for="(task, taskIndex) in column.tasks" :key="taskIndex" 
                    :task="task"
                    :draggable="draggable"
                    @dragstart="onDragStart($event, columnIndex, taskIndex)"
                    @dragover.prevent
                    @drop="onTaskDrop($event, columnIndex, taskIndex)"
                    @dragenter="onDragEnter($event, columnIndex, taskIndex)"
                />
                <!-- @drop="onDropTask($event, taskIndex)" -->
        </div>

<!-- Мод-окно создания задачи -->
    <my-dialog v-model:show="dialogVisible_4" class="sel">
      <input v-model="taskTitle" type="text" placeholder="Название задачи" class="sel__input">
      <textarea v-model="taskDescr" id="" cols="30" rows="10" class="sel__input"></textarea>
      <my-button class="sel__btn" @click="createTask">Создать</my-button>
    </my-dialog>

</template>

<script>
  import ColumnTask from '@/components/ColumnTask.vue'  

export default {
    

    components: {
        ColumnTask
    },

    props: {
        
        selectedProjectSpace: {
            type: Object,
            default: null
        },
        
        selectedColumn: {
            type: Object,
            default: null
        }
    }, 

    data() {
        return {
            dialogVisible_4: false,
            selectedColumn: null,
            draggable: false,
            draggetTask: null,
            draggetColumn: null
        }
    },

   methods: {
    openTaskForm(columnValue) {
        this.selectedColumn = columnValue
        this.dialogVisible_4 = true
      },
      
// Добавление задач
      createTask() {
        
        this.dialogVisible_4 = false
        
        const newTask = {
          id: Date.now(),
          title: this.taskTitle,
          descr: this.taskDescr,
        }
        
        this.selectedColumn.tasks.push(newTask)
        this.taskTitle = ''
        this.taskDescr = ''
        
      },

      onDragStart(event, fromColumnIndex, fromTaskIndex) {
            event.dataTransfer.setData('fromColumnIndex', fromColumnIndex)
            event.dataTransfer.setData('fromTaskIndex', fromTaskIndex)
            event.dataTransfer.dropEffect = 'move'
            event.dataTransfer.effectAlloved = 'move'
        },

    onDrop(event, toColumnIndex) {
        const columns = this.selectedProjectSpace.columns
        const tasks = this.selectedColumn.tasks
        const fromColumnIndex = event.dataTransfer.getData('fromColumnIndex')
        const fromTaskIndex = event.dataTransfer.getData('fromTaskIndex')
        
        if(fromColumnIndex !== null && fromTaskIndex !== null) {
            const taskToMove = columns[fromColumnIndex].tasks.splice(fromTaskIndex, 1)[0]
            if(!taskToMove) return
            columns[toColumnIndex].tasks.push(taskToMove)
        } 


    },

        onTaskDrop(event, columnIndex, taskIndex) {
            const tasks = this.selectedColumn.tasks
            const columns = this.selectedProjectSpace.columns
            const fromColumnIndex = event.dataTransfer.getData('fromColumnIndex')
            const fromTaskIndex = event.dataTransfer.getData('fromTaskIndex')

            if(fromColumnIndex !== null && fromTaskIndex !== null) {
                const taskToMove = columns[fromColumnIndex].tasks[fromTaskIndex]
                if(!taskToMove) return

                if(fromColumnIndex === columnIndex) {
                    columns[columnIndex].tasks.splice(taskIndex, 0, columns[columnIndex].tasks.splice(fromTaskIndex, 1)[0])
                } else {
                    columns[fromColumnIndex].tasks.splice(fromTaskIndex, 1)
                    columns[columnIndex].tasks.splice(taskIndex, 0, taskToMove)
                }
            }
        },

        onDragEnter(event) {
            event.dataTransfer.dropEffect = 'move'
            event.preventDefault()
        }

   } 

}
</script>

<style>
.column {
    display: flex;
    flex-direction: column;
    align-items: center;
    border: 1px solid black;
    border-radius: 5px;
    width: 300px;
    min-height: 200px;
    background-color: #fbf0e1;
    padding: 0 10px;
    margin-right: 20px;
}
.column__title {
    width: 100%;
    margin: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.column__title_btn {
    padding: 2px;
    line-height: 10px;
    border: 1px solid black;
    border-radius: 3px;
    cursor: pointer;
}
.column__title_btn:hover {
    background-color: aquamarine;
    border: 1px solid aquamarine;
    border-radius: 3px;
}

</style>