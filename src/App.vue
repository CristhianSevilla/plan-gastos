<script setup>
import { ref, reactive, watch, computed, onMounted } from "vue";
import Presupuesto from "./components/Presupuesto.vue";
import ControlPresupuesto from "./components/ControlPresupuesto.vue";
import Modal from "./components/Modal.vue";
import Gasto from "./components/Gasto.vue";
import Filtros from "./components/Filtros.vue";
import iconoNuevoGasto from "./assets/img/nuevo-gasto.svg";
import { generarId } from "./helpers";

const presupuesto = ref(0);
const disponible = ref(0);
const gastos = ref([]);
const gastado = ref(0);
const filtro = ref("");

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

watch(
  gastos,
  () => {
    const totalGastado = gastos.value.reduce(
      (total, gasto) => gasto.cantidad + total,
      0
    );
    gastado.value = totalGastado;
    disponible.value = presupuesto.value - totalGastado;

    //Alamacenar los gastos en LS
    localStorage.setItem("gastos", JSON.stringify(gastos.value));
  },
  {
    deep: true,
  }
);

watch(
  modal,
  () => {
    if (!modal.mostrar) {
      reiniciarModal();
    }
  },
  {
    deep: true,
  }
);

//Este watch de presupuesto no tiene un deep, porque no es un objeto
watch(presupuesto, () => {
  //ponemos el presupuesto en el LS
  localStorage.setItem("presupuesto", presupuesto.value);
});

onMounted(() => {
  //Obtenemos el presupuesto del LS
  const presupuestoStorage = localStorage.getItem("presupuesto");
  //Si existe un presupuesto lo asignamos al State de presupuesto
  if (presupuestoStorage) {
    presupuesto.value = Number(presupuestoStorage);
    disponible.value = Number(presupuestoStorage);
  }

  //Colocar los gastos del LS en el state de gastos
  const gastosStorage = localStorage.getItem("gastos");
  if (gastosStorage) {
    gastos.value = JSON.parse(gastosStorage);
  }
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
  if (gasto.id) {
    //Editar Gasto
    const { id } = gasto;
    //buscamos el gasto a editar por su id
    const i = gastos.value.findIndex((gasto) => gasto.id === id);
    //creamos una copia de gasto y agregamos el objeto nuevo
    gastos.value[i] = { ...gasto };
  } else {
    //Gato Nuevo
    gastos.value.push({
      ...gasto,
      id: generarId(),
    });
  }

  ocultarModal();
  reiniciarModal();
};

const reiniciarModal = () => {
  //Reiniciar State de Gasto
  Object.assign(gasto, {
    nombre: "",
    cantidad: "",
    categoria: "",
    id: null,
    fecha: Date.now(),
  });
};

const seleccionarGasto = (id) => {
  const gastoEditar = gastos.value.filter((gasto) => gasto.id === id)[0];
  Object.assign(gasto, gastoEditar);
  mostrarModal();
};

const eliminarGasto = () => {
  if (confirm("¿Deseas eliminar el gasto?")) {
    gastos.value = gastos.value.filter(
      (gastoState) => gastoState.id !== gasto.id
    );
    ocultarModal();
  }
};

const gastosFiltrados = computed(() => {
  if (filtro.value) {
    return gastos.value.filter((gasto) => gasto.categoria === filtro.value);
  }
  return gastos.value;
});
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
        <Filtros v-model:filtro="filtro" />

        <div class="listado-gastos contenedor">
          <h2>{{ gastosFiltrados.length > 0 ? "" : "No hay Gastos" }}</h2>

          <Gasto
            v-for="gasto in gastosFiltrados"
            :key="gasto.id"
            :gasto="gasto"
            @seleccionar-gasto="seleccionarGasto"
          />
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
          @eliminar-gasto="eliminarGasto"
          :modal="modal"
          :disponible="disponible"
          :id="gasto.id"
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
  --anaranjado: #df4410;
  --anaranjado-oscuro: #ff6f00;
  --verde: #398b13;
  --blanco: #fff;
  --blanco-transparente: #ffffffd6;
  --gris-claro: #e6e6e6;
  --gris: #94a3b8;
  --gris-oscuro: #64748b;
  --negro: #000000;
  --negro-transparente: #000000d8;
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
  background-color: var(--negro-transparente);
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
  background-image: url("/src/assets/img/fondo.jpg");
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  position: fixed;
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
}
.contenido-con-scroll {
  overflow-y: scroll;
  height: 100vh;
  max-height: 100vh;
  background-color: #0000002c;
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
.sombra {
  box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
  background-color: var(--negro);
  border-radius: 1.2rem;
  padding: 4rem;
  width: 90%;
  max-width: 70rem;
  margin: 0rem auto -18rem auto;
}
@media (min-width: 768px) {
  .sombra {
    max-width: 70rem;
    margin: 0rem auto -15rem auto;
  }
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
  margin-top: 20rem;
}
.listado-gastos h2 {
  font-weight: 900;
  color: var(--blanco);
  text-align: center;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}
</style>
