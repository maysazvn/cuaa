<script setup>
import { useRoute} from 'vue-router'
import { salasUsuario } from '@/data/salasUsuario'
import { salas } from '@/data/salas'
import { computed } from 'vue'
const route = useRoute()
const salaa = computed(() => salas.value.find((s) => s.idSala == route.params.id))

const props = defineProps({
  sala: {
    type: Object,
    required: true
  }
})

function estaNaSala() {
  return salasUsuario.value.some((i) => i.idSala === props.sala.idSala)
}

function entrar() {
  if (!estaNaSala()) {
    salasUsuario.value.push(props.sala)
    salaa.value.participantes++
  }
}

function sair() {
  salasUsuario.value = salasUsuario.value.filter((i) => i.idSala !== props.sala.idSala)
  salaa.value.participantes--
}
</script>

<template>
  <button v-if="estaNaSala()" @click="sair" class="btn-sair">
    Sair
  </button>
  <button v-else @click="entrar" class="btn-entrar">
    Entrar
  </button>
</template>

<style scoped>
.btn-entrar,
.btn-sair {
  font-weight: bold;
  font-size: 0.95rem;
  padding: 5px 15px;
  border-radius: 25px;
  border: none;
  cursor: pointer;
  transition: all 0.2s ease;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.btn-entrar {
  background: #F8D76B;
  color: #1e1e1e;
}

.btn-sair {
  background-color: #3c3c3c;
  color: #d9d9d9;
}

.btn-sair:hover {
  background-color: #e53935;
  color: #ffffff;
}

button:hover {
  opacity: 0.9;
  transform: scale(0.95);
}
</style>