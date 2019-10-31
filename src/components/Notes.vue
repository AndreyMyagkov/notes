<template>
    <!-- note-list -->
    <div class="notes">
        <div class="note" 
            :class="[{ full: !grid}, 'priority_'+(note.priority?note.priority:1)]" 
            v-for="(note, index) in notes"
            :id="`note-${index}`"
            :key="index">

            <div class="note-header" :class="{ full: !grid}">
                
                <!-- title -->
                <div >
                    <div @click="editTitle(index)" v-if="editedField!='title' || index!=editedId">{{ note.title }}</div>
                    <input class="title__edit-area"
                        v-if="editedField=='title' && index==editedId"
                        v-model="note.title"
                        :id="`note-${index}-title`"
                        @blur="saveTitle(index)"
                        v-on:keyup.enter="saveTitle(index)"
                        v-on:keyup.esc="rollbackTitle(index)"
                    >
                    
                </div>

                <div style="cursor:pointer;" @click="removeNote(index)">x</div>
            </div>

            <!-- descr  -->
            <div class="note-body">
                <p @click="editDescr(index)" v-if="editedField!='descr' || index!=editedId">{{note.descr}}</p>
                <input class="title__edit-area"
                    v-if="editedField=='descr' && index==editedId"
                    v-model="note.descr"
                    :id="`note-${index}-descr`"
                    @blur="saveDescr(index)"
                    v-on:keyup.enter="saveDescr(index)"
                    v-on:keyup.esc="rollbackDescr(index)"
                >
            </div>

            <!-- date  -->
            <div class="note-date">
                <p>{{note.date}}</p>
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
        },
        editedId:{
            validator: prop => typeof prop === 'number' || prop === null, // Число или null
            required: true

        },
        editedField: {
            type: String,
            required: true 
        }
    },
    data(){
        return {
          
        }
    },
    methods:{
        removeNote(index){
            console.log(`Note id ${index} - removed`);
            this.$emit('remove', index)
        },
        // Заголовки
        editTitle(index){
             this.$emit('editTitle', index);
         },
        saveTitle(index){
            this.$emit('saveTitle', index)
        },
        rollbackTitle(index){
            this.$emit('rollbackTitle', index);
        },

        // Описание
        editDescr(index){
             this.$emit('editDescr', index);
         },
        saveDescr(index){
            this.$emit('saveDescr', index)
        },
        rollbackDescr(index){
            this.$emit('rollbackDescr', index);
        },
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
        margin-bottom: 30px;
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
        margin-bottom: 30px;
        p{
            font-size: 18px;
            color:#333;
           
        }
        
    }

    .note-date {
        p{
            font-size: 14px;
            color:#999;
        }
        
    }
    .title__edit-area {
        padding:0;
        margin:0;
        border-radius:3px;
        padding: 0 0.25em;
        height: 28px;
        font-size: 16px;
    }

</style>