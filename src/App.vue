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
                <notes :notes="notesFiltered" :grid="grid" @remove="removeNote"/>
  
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
        }
    }

};
</script>

<style>
</style>
