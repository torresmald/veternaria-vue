<script setup>
import Alerta from "./Alerta.vue";
import { ref, reactive } from "vue";

const emits = defineEmits([
  "update:nombre",
  "update:propietario",
  "update:email",
  "update:fecha",
  "update:sintomas",
  "guardar-paciente",
]);

const paciente = defineProps({
  nombre: String,
  propietario: String,
  email: String,
  fecha: String,
  sintomas: String,
  required: true,
});
const alerta = reactive({
  tipo: "",
  mensaje: "",
});

const validarFormulario =  () => {
  if (Object.values(paciente).includes("")) {
    alerta.mensaje = "Todos los Campos son Obligatorios";
    alerta.tipo = "error";
    return;
  } else {
    alerta.mensaje = "Guardado Correctamente";
    alerta.tipo = "";
    emits("guardar-paciente");
    setTimeout(() => {
        alerta.mensaje = ''
        alerta.tipo = ''
    }, 2000)
  }
};
</script>
<template>
  <div class="md:w-1/2">
    <h2 class="text-3xl font-black text-center">Seguimiento Pacientes</h2>
    <p class="text-l mb-10 mt-10 text-center font-black">
      Añade Pacientes y
      <span class="text-emerald-600 font-bold">Adminístralos</span>
    </p>
    <form
      class="shadow-md bg-white py-10 px-5 rounded-lg mb-10"
      @submit.prevent="validarFormulario"
    >
      <Alerta :alerta="alerta" v-if="alerta.mensaje" />
      <div class="mb-5">
        <label for="mascota" class="font-bold text-gray-700 uppercase block"
          >Nombre Mascota</label
        >
        <input
          type="text"
          id="mascota"
          class="border-2 w-full p-2 mt-2 rounded-md placeholder:text-grey-400"
          placeholder="Nombre de la Mascota"
          @input="$emit('update:nombre', $event.target.value)"
          :value="nombre"
        />
      </div>
      <div class="mb-5">
        <label for="propietario" class="font-bold text-gray-700 uppercase block"
          >Propietario</label
        >
        <input
          type="text"
          id="propietario"
          class="border-2 w-full p-2 mt-2 rounded-md placeholder:text-grey-400"
          placeholder="Nombre del Propietario"
          @input="$emit('update:propietario', $event.target.value)"
          :value="propietario"
        />
      </div>
      <div class="mb-5">
        <label for="email" class="font-bold text-gray-700 uppercase block"
          >Email</label
        >
        <input
          type="email"
          id="email"
          class="border-2 w-full p-2 mt-2 rounded-md placeholder:text-grey-400"
          placeholder="Email de Contacto"
          @input="$emit('update:email', $event.target.value)"
          :value="email"
        />
      </div>
      <div class="mb-5">
        <label for="alta" class="font-bold text-gray-700 uppercase block"
          >Fecha</label
        >
        <input
          type="date"
          id="alta"
          class="border-2 w-full p-2 mt-2 rounded-md"
          @input="$emit('update:fecha', $event.target.value)"
          :value="fecha"
        />
      </div>
      <div class="mb-5">
        <label for="sintomas" class="font-bold text-gray-700 uppercase block"
          >Sintomas</label
        >
        <textarea
          id="sintomas"
          class="border-2 w-full p-2 mt-2 rounded-md placeholder:text-grey-400 h-40"
          placeholder="Describe los Sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
          :value="sintomas"
        />
      </div>
      <input
        type="submit"
        class="bg-emerald-600 text-white uppercase px-5 py-3 rounded-lg w-full hover:cursor-pointer hover:bg-emerald-800 text-xl"
        value="Registrar Paciente"
      />
    </form>
  </div>
</template>
