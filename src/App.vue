<script setup>
import { ref, reactive, watch, onMounted } from "vue";
import { uid } from "uid";
import Header from "./components/Header.vue";
import Formulario from "./components/Formulario.vue";
import Paciente from "./components/Paciente.vue";

const pacientes = ref([]);

const paciente = reactive({
  id: null,
  nombre: "",
  propietario: "",
  email: "",
  fecha: "",
  sintomas: "",
});

const agregarLocalStorage = () => {
  localStorage.setItem('pacientes', JSON.stringify(pacientes.value));
}
const recuperarLocalStorage = () => {
  const pacientesStorage = localStorage.getItem("pacientes");
  if (pacientesStorage) {
    pacientes.value = JSON.parse(pacientesStorage);
  }
}
onMounted (() => {
  recuperarLocalStorage()
})

watch(
  pacientes,
  () => {
    agregarLocalStorage();
  },
  {
    deep: true,
  }
);

const resetearFormulario = () => {
  paciente.id = null;
  paciente.nombre = "";
  paciente.propietario = "";
  paciente.email = "";
  paciente.fecha = "";
  paciente.sintomas = "";
};
const guardarPaciente = () => {
  if (paciente.id) {
    const editarPaciente = pacientes.value.findIndex(
      (element) => element.id === paciente.id
    );
    pacientes.value[editarPaciente] = { ...paciente };
  } else {
    pacientes.value.push({
      ...paciente,
      id: uid(),
    });
  }
  resetearFormulario();
};

const editarPaciente = (id) => {
  const pacientesEditar = pacientes.value.filter(
    (paciente) => paciente.id === id
  )[0];
  Object.assign(paciente, pacientesEditar);

};

const eliminarPaciente = (id) => {
  pacientes.value = pacientes.value.filter((paciente) => paciente.id !== id)
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario
        :pacientes="pacientes"
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:fecha="paciente.fecha"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll font-black">
        <h3 class="text-2xl text-center">Administra tus Pacientes</h3>
        <p class="text-l mb-10 mt-10 text-center">
          Información sobre
          <span class="text-emerald-600 font-bold">Pacientes</span>
        </p>
        <div v-if="pacientes.length">
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @editar-paciente="editarPaciente(paciente.id)"
            @eliminar-paciente="eliminarPaciente(paciente.id)"
          />
        </div>
        <p v-else class="mt-20 text-center text-2xl">No hay Pacientes</p>
      </div>
    </div>
  </div>
</template>
