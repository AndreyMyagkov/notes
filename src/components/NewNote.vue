<template>
    <div class="new-note">
        <div class="new-note__fields">
            <div class="new-note__title">
                <label for="">Title</label>
                <input type="text" v-model="note.title" />
            </div>
            <div class="new-note__priority">
                <label for="">priority</label>
                <select v-model="note.priority">
                    <option value="1">low</option>
                    <option value="2">important</option>
                    <option value="3">critical</option>
                </select>
            </div>
        </div>
    <label for="">Description</label>
    <textarea name id cols="30" rows="10" v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New note</button>
    </div>
</template>

<script>
export default {
    props: {
        note:{
            type: Object,
            required: true
        }
    },
    methods:{
        addNote(){
            this.$emit('addNote', this.note);

        }
    }
}
</script>

<style lang="scss">
    .new-note {
        text-align: center;
        margin-bottom: 20px;

    }
    .new-note__fields {
        display: flex;
        justify-content: space-between;
    }
    .new-note__title,
    .new-note__priority {
        flex: 1 1 48%;
        max-width: 48%;
    }
</style>