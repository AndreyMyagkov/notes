<template>
  <div class="wrapper">
    <div class="wrapper-content">


        <section>
            <div class="container">
               

                <message v-if="message" :message="message"/>

                <!-- new -->
                <newNote 
                    :note="note"
                    @addNote="addNote"/>
 
                <!-- title -->
                <div class="note-header" style="margin: 36px 0">
                    <h1>{{title}}</h1>

                    <!-- search  -->
                  
                    <search
                        :value="search"
                        @search="search = $event"
                        placeholder="Find your note" />


                    <div class="icons">
                        <svg :class="{ active: grid }"
                         @click="grid= true"
                         style="cursor: pointer;" 
                          xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>

                        <svg :class="{ active: !grid }"
                          @click="grid= false"
                          style="cursor: pointer;"
                           xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>

                    </div>
                </div>

                <!-- list -->
                <notes
                    :notes="notesFiltered" 
                    :grid="grid" 
                    :editedId="editor.id"
                    :editedField="editor.field"
                    @remove="removeNote" 

                    @editTitle="editTitle"
                    @saveTitle="saveTitle"
                    @rollbackTitle="rollbackTitle"

                    @editDescr="editDescr"
                    @saveDescr="saveDescr"
                    @rollbackDescr="rollbackDescr"
                />
  
            </div>
        </section>


    </div>
  </div>
</template>

<script>
import message from "@/components/Message.vue";
import notes from "@/components/Notes.vue";
import newNote from "@/components/NewNote.vue";
import search from "@/components/Search.vue";
export default {
    components:{
        message, notes, newNote, search
    },
    data(){
        return {
            title: 'Note App',
            search:'',
            note: {
                title:'',
                descr:'',
                priority: 1
            },
            editor: {           // Данные редактора
                id: null,       // id редактируемой записи. Nul - если ничего не радактируется
                field: '',      // Редактируемое поле
                oldTitle:'',    // Предыдущее значение Title
                oldDescr:''     // Предыдущее значение Descr
            },
            message: null,
            grid: true,
            notes: [
                {
                    title: 'First Note',
                    descr: 'Description for first note',
                    date: new Date(Date.now()).toLocaleString(),
                },
                {
                    title: 'Second Note',
                    descr: 'Description for second note',
                    date: new Date(Date.now()).toLocaleString()
                },
                {
                    title: 'Third Note',
                    descr: 'Description for third note',
                    date: new Date(Date.now()).toLocaleString()
                }
            ]
        }
    },
    computed: {
        notesFiltered(){
            let array = this.notes,
                search = this.search;
            if (!search) {return  array}
            // Small
            search = search.trim().toLocaleString();
            // Filter
            array=array.filter(function(item){
                if (item.title.toLowerCase().indexOf(search) !==-1){
                    return item
                }
            })
            // Error
            return array;
        }
    },
    methods:{
        addNote(){
          
            let {title, descr, priority}=this.note;

            if (title==="") { 
                this.message="Title can't be blank";
                return false}

            this.notes.push({
                title,
                descr,
                priority,
                date: new Date(Date.now()).toLocaleString()
            })
            this.note.title='';
            this.note.descr='';
            this.note.priority=1;
            this.message=null;
        },
        removeNote(index){
            this.notes.splice(index, 1);
        },

        /*
        * Активирует режим редактирования заголовка
        */
        editTitle(index){
            console.log('edit from root app');

            this.editor.id = index;
            this.editor.field = 'title';
            this.editor.oldTitle=this.notes[index].title;

            // Фокус в поле после отрисовки инпута
            setTimeout(
                function(){
                    document.querySelector(`#note-${index}-title`).focus();
                }, 0, index
            )
        },

        /*
        * Сохраняет заголовок
        */
        saveTitle(index){
            // Нельзя сохранить пустой заголовок
            if (this.notes[index].title===''){
                this.rollbackTitle(index);
                return
            }
            if (this.editor.id!= null  ){
                console.log(`Note id ${index} - save Title`);
                this.editor.id = null;
                this.editor.field = '';
                this.editor.oldTitle='';
                this.updateDate(index);
            }
        },

        /*
        * Откатывает заголовок
        */
        rollbackTitle(index){
            console.log(`Note id ${index} - rollback Title`);
            this.notes[index].title = this.editor.oldTitle;
            this.editor.oldTitle='';
            this.editor.id = null;
            this.editor.field = '';
        },

        /*
        * Обновление даты заметки
        */
        updateDate(index){
            this.notes[index].date=new Date(Date.now()).toLocaleString();
        },



         /*
        * Активирует режим редактирования описания
        */
        editDescr(index){
            console.log('edit from root app');

            this.editor.id = index;
            this.editor.field = 'descr';
            this.editor.oldDescr=this.notes[index].descr;

            // Фокус в поле после отрисовки инпута
            setTimeout(
                function(){
                    document.querySelector(`#note-${index}-descr`).focus();
                }, 0, index
            )
        },

        /*
        * Сохраняет описание
        */
        saveDescr(index){
            // Нельзя сохранить пустое описание
            if (this.notes[index].descr===''){
                this.rollbackDescr(index);
                return
            }
            if (this.editor.id!= null  ){
                console.log(`Note id ${index} - save Descr`);
                this.editor.id = null;
                this.editor.field = '';
                this.editor.oldDescr='';
                this.updateDate(index);
            }
        },

        /*
        * Откатывает описание
        */
        rollbackDescr(index){
            console.log(`Note id ${index} - rollback Descr`);
            this.notes[index].descr = this.editor.oldDescr;
            this.editor.oldDescr='';
            this.editor.id = null;
            this.editor.field = '';
        },
    }

};
</script>

<style>
</style>
