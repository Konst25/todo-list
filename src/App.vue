<template>
  <div class="app">

    <!-- Header-menu -->
    <header class="header">
      <div class="logo">ToDo list</div>

      <!-- Выбор рабочего пространства -->
      <my-select
        v-model="selectedProject"
        :projects="projects"
        @change="handleChangeOption"
      />

      <my-button class="create__btn" @click="selCreate">Создать</my-button>
      <div class="user">User</div>
    </header>
    
<!-- Рабочие проекты -->
    <div v-if="selectedProject">
      
      <work-space
        :selectedProjectSpace="selectedProjectSpace"
        @remove="removeProject"
      >
        <div class="work__space">

          <column-item
            :selectedProjectSpace="selectedProjectSpace"
            :selectedColumn="selectedColumn"
            @remove="removeColumn"
                      
          />

        </div> 

      </work-space>

    </div>
    
    <div class="no__project" v-else :show="dialogVisible_3 = false">
      <h2>Создайте или выберите рабочий проект</h2>
    </div>
    
<!-- --------Модальные окна--------  -->

    <!-- Мод-окно выбора кнопки создания -->
    <my-dialog v-model:show="dialogVisible" class="sel">
      <my-button class="sel__btn" @click="openForm_1">Создать рабочий проект</my-button>
      <my-button v-model:show="dialogVisible" @click="openForm_2" style="margin-top: 10px;" class="sel__btn">Создать колонку</my-button>
    </my-dialog>

    <!-- Мод-окно создания раб. проекта -->
    <my-dialog v-model:show="dialogVisible_2" class="sel">
      <input v-model="titleProject" type="text" placeholder="Название проекта" class="sel__input">
      <my-button class="sel__btn" @click="createWorkProject">Создать</my-button>
    </my-dialog>

  <!-- Мод-окно создания колонки -->
    <my-dialog v-model:show="dialogVisible_3" class="sel">
      <input v-model="columnTitle" type="text" placeholder="Название колонки" class="sel__input">
      <my-button class="sel__btn" @click="createColumn">Создать</my-button>
    </my-dialog>

  
<!-- --------Модальные окна---------- -->
    
    
  </div>
</template>

<script>

// Импорт компонентов из папки components
import ColumnItem from '@/components/ColumnItem'
import WorkSpace from '@/components/WorkSpace'


export default {

  // Регистрация компонентов из папки components
  components: {
    ColumnItem,
    WorkSpace,
    
    
  },

  

  // Создание моделей
  data () {
    return {
      dialogVisible: false,
      dialogVisible_2: false,
      dialogVisible_3: false,
      
      selectedProject: '',
      selectedProjectSpace: null,
      draggable: false,
      projects: [],

      columnItems: [
          {id: 1, title: 'Колонка 1', tasks: []},
          {id: 2, title: 'Колонка 2', tasks: []},
          {id: 3, title: 'Колонка 3', tasks: []},
      ],
      
      ColumnTasks: [
        {id: 1, title: 'Задача 1', descr: 'Описание 1'},
        {id: 2, title: 'Задача 2', descr: 'Описание 2'},
        {id: 3, title: 'Задача 3', descr: 'Описание 3'},
      ],
      columnTitle: '',
      taskTitle: '',
      taskDescr: '',
      
    } 
  },

  computed: {

  },

  // Создание методов
  methods: {

    handleTaskChange(event) {
      console.log(event)
    },

  // Функция отображения модального окна кнопок создания
    selCreate() {
      this.dialogVisible = true
    }, 

// Открытие формы создания проекта 
    openForm_1() {
      this.dialogVisible = false
      this.dialogVisible_2 = true
    }, 

// Открытие формы создания колонки 
    openForm_2() {
      this.dialogVisible = false
      this.dialogVisible_3 = true
    },



    

// Функция создания рабочего проекта
    createWorkProject() {
      const newOption = {
        id: Date.now(),
        name: this.titleProject,
        value: '',
        columns: []
      }
      this.projects.push(newOption)
      this.dialogVisible_2 = false
      this.titleProject = ''
    },
// Отслеживание выбранного проекта
    handleChangeOption(event) {
      const projectId = parseInt(event.target.value)
          this.selectedProjectSpace = this.projects.find(project => 
            project.id === projectId
          )
    
        },
// Удаление рабочего проекта
    removeProject(ev) {
      this.projects = this.projects.filter(project => project.id !== ev.id)
      this.selectedProject = ''
    },

// Создание колонки задач
     createColumn() {
        const newColumn = {
              id: Date.now(),
              title: this.columnTitle,
              tasks: []
            }
            this.selectedProjectSpace.columns.push(newColumn)  
            this.dialogVisible_3 = false
            this.columnTitle = ''
            
        },
// Удаление колонки задач
      removeColumn(column) {
        this.selectedProjectSpace.columns = this.selectedProjectSpace.columns.filter(obj => obj.id !== column.id)
      },



     
  },

  
}
</script>

<style>
/* Классическое обнуление стилей body */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Добавление фонового изображения */
.app {
  background-color: #d3e7f9;
  height: 100vh; /* На высоту экрана */
  
}

/* Стили header-menu */
.header {
  display: flex;
  align-items: center;
  padding: 10px 20px;
  position: fixed;
  z-index: 99;
  width: 100%;
  min-height: 40px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}
.logo {
  background-color: #fbf0e1;
  padding: 5px 5px;
}
.user {
  width: 30px;
  height: 30px;
  padding: 5px 0;
  border-radius: 50%;
  background-color: #fbf0e1;
  margin-left: auto;
  margin-right: 10px;
  overflow: hidden;
}
.select {
  margin-left: 20px;
}

/* Стили модального окна */
.create__btn {
  background: #fbf0e1;
  margin-left: 20px;
}
.sel {
  
  flex-direction: column;
}

.sel__btn, .sel__input {
  margin-top: 10px;
  display: block;
}

.work__space h2 {
    position: fixed;
    right: 0;
    bottom: 90%;
    left: 50%;
    top: 70px;
    transform: translate(-50%, 0);
    margin: auto;
    font-size: 20px;
    text-align: center;
    
}
.work__space {
  display: flex;
  position: absolute;
  top: 110px;
  display: flex;
  
}

.no__project {
  position: absolute;
  width: 100%;
  top: 70px; 
  left: 0; 
  bottom: 0; 
  right: 0;
  margin: auto;
}
.no__project h2 {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 50%;
  text-align: center;
  transform: translate(-50%, 0);
}

</style>
