<template>
    <transition name="modal">
        <div class="modal" @click="clean">
            <div class="modal__content" @click.stop>
                <h3 class="modal__title">{{id ? lang[lg].edit : lang[lg].add}} {{lang[lg].note}}</h3>
                <form class="modal__form" @submit.prevent="send">
                    <input type="text" class="modal__input" required placeholder="Title" v-model="title">
                    <textarea class="modal__input modal__area" placeholder="Content" required v-model="text"></textarea>
                    <div class="modal__control">
                        <a href="#!" class="modal__cancel" @click.prevent="clean">{{lang[lg].cancel}}</a>
                        <button class="modal__btn">{{id ? lang[lg].edit : lang[lg].add}}</button>
                    </div>
                </form>
            </div>
        </div>
    </transition>
</template>
<script setup>
    import { computed } from '@vue/runtime-core';
    import {state, lang, addNote, editNote} from '../store';
    const lg = computed(() => state.value.curLang);
    const title = computed({
        get: () => state.value.modalTitle,
        set: val => state.value.modalTitle = val
    });
    const text = computed({
        get: () => state.value.modalDescr,
        set: val => state.value.modalDescr = val
    });
    const id = computed({
        get: () => state.value.curID,
        set: val => state.value.curID = val
    });
    function clean(){
        title.value = '';
        text.value = '';
        id.value = null;
        state.value.showModal = false;
    }
    function send(){
        const obj = {
            title: title.value,
            text: text.value,
            date: new Date().toLocaleString(),
            id: id.value || Date.now()
        };
        if(id.value) editNote(id.value, obj);
        else addNote(obj);
        clean();
    }
</script>