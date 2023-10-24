<script setup>
import { ref, reactive, watch } from "vue";
import Presupuesto from "./components/Presupuesto.vue";
import ControlPresupuesto from "./components/ControlPresupuesto.vue";
import Modal from "./components/Modal.vue";
import Gasto from "./components/Gasto.vue";
import iconoNuevoGasto from "./assets/img/nuevo-gasto.svg";
import { generarId } from "./helpers";

const presupuesto = ref(0);
const disponible = ref(0);
const gastos = ref([]);
const gastado = ref(0);

watch(
  gastos,
  () => {
    const totalGastado = gastos.value.reduce(
      (total, gasto) => gasto.cantidad + total,
      0
    );
    gastado.value = totalGastado;
    disponible.value = presupuesto.value - totalGastado;
  },
  {
    deep: true,
  }
);

const modal = reactive({
  mostrar: false,
  animar: false,
});
const gasto = reactive({
  nombre: "",
  cantidad: "",
  categoria: "",
  id: null,
  fecha: Date.now(),
});

const definirPresupuesto = (cantidad) => {
  presupuesto.value = cantidad;
  disponible.value = cantidad;
};

const mostrarModal = () => {
  modal.mostrar = true;
  setTimeout(() => {
    modal.animar = true;
  }, 300);
};
const ocultarModal = () => {
  setTimeout(() => {
    modal.mostrar = false;
  }, 500);
  modal.animar = false;
};

const guardarGasto = () => {
  gastos.value.push({
    ...gasto,
    id: generarId(),
  });

  ocultarModal();

  //Reiniciar Formulario
  Object.assign(gasto, {
    nombre: "",
    cantidad: "",
    categoria: "",
    id: null,
    fecha: Date.now(),
  });
};
</script>

<template>
  <div class="imagen-fondo">
    <div class="contenido-con-scroll">
      <header>
        <div class="contenedor-header">
          <h1>Planificador de Gastos</h1>

          <div class="sombra">
            <Presupuesto
              v-if="presupuesto === 0"
              @definir-presupuesto="definirPresupuesto"
            />
            <ControlPresupuesto
              v-else
              :presupuesto="presupuesto"
              :disponible="disponible"
              :gastado="gastado"
            />
          </div>
        </div>
      </header>

      <main v-if="presupuesto > 0">
        <div class="listado-gastos contenedor">
          <h2>{{ gastos.length > 0 ? "Gastos" : "No hay Gastos" }}</h2>

          <Gasto v-for="gasto in gastos" :key="gasto.id" :gasto="gasto" />
        </div>

        <div class="crear-gasto">
          <img
            :src="iconoNuevoGasto"
            @click="mostrarModal"
            alt="Icono Nuevo Gasto"
          />
        </div>

        <Modal
          v-if="modal.mostrar"
          @ocultar-modal="ocultarModal"
          @guardar-gasto="guardarGasto"
          :modal="modal"
          v-model:nombre="gasto.nombre"
          v-model:cantidad="gasto.cantidad"
          v-model:categoria="gasto.categoria"
        />
      </main>
    </div>
  </div>
</template>

<style>
:root {
  --azul: #3167be;
  --verde: #398b13;
  --amarillo: #dab811;
  --rosa: #c21f68;
  --blanco: #fff;
  --gris-claro: #e6e6e6;
  --gris: #94a3b8;
  --gris-oscuro: #64748b;
  --negro: #000;
}
html {
  font-size: 62.5%;
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}
body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
}
h1 {
  font-size: 4rem;
}
h2 {
  font-size: 3rem;
}
header {
  background-color: var(--azul);
}
header h1 {
  width: 100%;
  padding: 3rem 0;
  margin: 0;
  color: var(--blanco);
  text-align: center;
  text-transform: capitalize;
  font-size: 3rem;
}
.imagen-fondo {
  background-image: url("/src/assets/img/2.svg");
  background-size: cover;
  background-position: bottom right;
  background-repeat: no-repeat;
  position: fixed;
  width: 100%;
  height: 100vh;
}
.contenido-con-scroll {
  overflow-y: scroll;
  height: 100vh;
  max-height: 100vh;
}
.contenedor {
  width: 90%;
  max-width: 80rem;
  margin: 0 auto;
}
.contenedor-header {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
@media (min-width: 768px) {
}
.sombra {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--blanco);
  border-radius: 1.2rem;
  padding: 4rem;
  width: 90%;
  max-width: 70rem;
  margin: 0rem auto -15rem auto;
}
.crear-gasto {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
  animation: pulse 2s infinite;

  padding: 0;
}
.crear-gasto img {
  width: 5rem;
  margin: 0;
  border: 5px solid var(--blanco);
  border-radius: 50%;
  transition: transform 0.3s ease;
}
.crear-gasto img:hover {
  cursor: pointer;
  transform: scale(1.2);
}

.listado-gastos {
  margin-top: 15rem;
}
.listado-gastos h2 {
  font-weight: 900;
  color: var(--gris-claro);
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}
</style>
