<template>
  <Header />
  <Notes :notes="filterNOtes"/>
  <Teleport to="body" >
    <Modal v-show="state.showModal">
    </Modal>
  </Teleport>
  <a href="#!" class="add__note" @click.prevent="state.showModal = true">
    <img src="@/assets/img/add-note.svg" alt="">
  </a>
</template>
<script setup>
  import '@/assets/styles/main.scss';
  import Header from './components/Header.vue';
  import Notes from './components/Notes.vue';
  import Modal from './components/Modal.vue';
  import {state} from './store';
import { computed } from '@vue/runtime-core';
  const filterNOtes = computed(() => {
    return state.value.search ? 
    state.value.notes.filter(c => c.title.toLowerCase().includes(state.value.search.toLowerCase()))
    : state.value.notes
  })
</script>