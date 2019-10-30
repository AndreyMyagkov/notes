<template>
    <!-- note-list -->
    <div class="notes">
        <div class="note" 
            :class="[{ full: !grid}, 'priority_'+(note.priority?note.priority:1)]" 
            v-for="(note, index) in notes"
            :id="`note-${index}`"
            :key="index">

            <div class="note-header" :class="{ full: !grid}">
                
                <div  :class="{isEdited: index==editedId}">
                    <div @click="editTitle(index)">{{ note.title }}</div>
                    <input class="title__edit-area"
                    v-model="note.title"
                    @blur="saveTitle(index)"
                    v-on:keyup.esc="rollbackTitle(index)"
                    v-on:keyup.enter="saveTitle(index)"
                 
                    style="padding:0; margin:0; border-radius:3px; padding: 3px 0.251em">
                    
                </div>

                <div style="cursor:pointer;" @click="removeNote(index)">x</div>
            </div>
            <div class="note-body">
            <p>{{note.descr}} {{note.date}}</p>
            </div>
        </div>
    </div>   
</template>

<script>
export default {
    props: {
        notes: {
            type: Array,
            required: true
        },
        grid: {
            type: Boolean,
            required: true            
        }
    },
    data(){
        return {
            editedId:null, // id редактируемой записи
            oldTitleValue:'', // предыдущее значение
        }
    },
    methods:{
        removeNote(index){
            console.log(`Note id ${index} - removed`);
            this.$emit('remove', index)
        },
        editTitle(index){ //??
            console.log(`Note id ${index} - edit Title`);
            this.oldTitleValue=this.notes[index].title;
            this.editedId = index;
            console.log('#note-'+index+' input focus');

            // Фокус в поле
            setTimeout(
                function(index){
                    document.querySelector('#note-'+index+' input').focus();
                }, 0, index
            )
          

           // this.$refs['title-'+index].focus();
        },
        updateData(index){
            this.notes[index].date=new Date(Date.now()).toLocaleString();
        },
        rollbackTitle(index){
            console.log(`Note id ${index} - rollback Title`);
            this.notes[index].title = this.oldTitleValue;
            this.oldTitleValue='';
            this.editedId = null;
        },
        saveTitle(index){
            console.log(`Note id ${index} - save Title`);
            //this.notes[index].title = this.oldTitleValue;
            if (this.editedId!= null){
                this.oldTitleValue='';
                this.editedId = null;
                this.updateData(index);
            }
        },
        hideOnClickOutside(element) {
          const outsideClickListener = event => {
              if (event.target !== element && !element.contains(event.target)){
                  
                 // this.saveTitle(this.index)
              }
          }
          document.addEventListener('click', outsideClickListener)
      }
    },
    mounted()  {      
    //  this.hideOnClickOutside(document.querySelector('#note-1'));
     // this.hideOnClickOutside(document.querySelector('#note-2'));
     // this.hideOnClickOutside(document.querySelector('body > div > div > section > div > div.notes > div:nth-child(2)'));
    }

}
</script>

<style lang="scss">
    .notes {
        display:flex;
        align-items:  center;
        justify-content: space-between;
        flex-wrap: wrap;
        padding: 40px 0;
    }
    .note {
        width: 48%;
        padding: 18px 20px;
        margin-bottom: 20px;
        background-color: #fff;
        transition: all 0.25s cubic-bezier(0.075, 0.82, 0.165, 1);
        box-shadow: 0 30px 30px rgba(0,0,0,0.02);
        &:hover{
            box-shadow: 0 30px 30px rgba(0,0,0,0.04);
            transform: translate(0, -6px);
            transition-delay: 0s !important;
        }
        &.full {
            width:100%;
            text-align: center;
        }
    }
    .priority_2{
        border:solid 2px #eae015;
    }
    .priority_3{
        border:solid 2px #e92b2b
    }
    .note-header {
        display:flex;
        align-items:  center;
        justify-content: space-between;
        h1 {
            font-size :32px;
        }

        >div {
            color: #402caf;
            font-size: 20px;
        }

        svg {
            margin-right: 12px;
            color:#999;
            &:last-child {
                margin-right:0;
            }
            &.active {
                color:#402caf;
            }
        }
        &.full {
            justify-content: center;
            p{
                margin-right: 16px;
                &:last-child {
                    margin-right: 0;
                }
            }
        }
    }
    .note-body {
        p{
            font-size: 14px;
            color:#999;
        }
        
    }
    .isEdited>div{
        display: none;
    }
    .note input,
    .note textarea{
        display: none;
    }
    .isEdited>input,
    .isEdited>textarea{
        display: block;
    }
</style>