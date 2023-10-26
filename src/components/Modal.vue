<script setup>
import { ref } from "vue";
import Alerta from "./Alerta.vue";
import cerrarModal from "../assets/img/cerrar.svg";

const error = ref("");

const emit = defineEmits([
  "ocultar-modal",
  "guardar-gasto",
  "update:nombre",
  "update:cantidad",
  "update:categoria",
]);

const props = defineProps({
  modal: {
    type: Object,
    required: true,
  },
  nombre: {
    type: String,
    required: true,
  },
  cantidad: {
    type: [String, Number],
    required: true,
  },
  categoria: {
    type: String,
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
  id: {
    type: [String, null],
    required: true,
  },
});

const cantidadOld = props.cantidad;

const agregarGasto = () => {
  //Validar que no existan campos vacios
  const { nombre, cantidad, categoria, disponible, id } = props;
  if ([nombre, cantidad, categoria].includes("")) {
    error.value = "Todos los campos son requeridos";

    setTimeout(() => {
      error.value = "";
    }, 3000);

    return;
  }
  //Validar Cantidad
  if (cantidad <= 0) {
    error.value = "Cantidad no válida";

    setTimeout(() => {
      error.value = "";
    }, 3000);

    return;
  }

  //Validar que el usuario no gaste más de lo disponible
  if (id) {
    if (cantidad > cantidadOld + disponible) {
      error.value = "Has excedido el presupuesto";

      setTimeout(() => {
        error.value = "";
      }, 3000);

      return;
    }
  } else {
    if (cantidad > disponible) {
      error.value = "Has excedido el presupuesto";

      setTimeout(() => {
        error.value = "";
      }, 3000);

      return;
    }
  }

  emit("guardar-gasto");
};

//Texto condicional del formulario
const texto = {
  encabezado: "",
  boton: "",
};
if (props.id) {
  Object.assign(texto, {
    encabezado: "Editar Gasto",
    boton: "Guardar Cambios",
  });
} else {
  Object.assign(texto, {
    encabezado: "Nuevo Gasto",
    boton: "Agregar Gasto",
  });
}
</script>
<template>
  <div class="modal">
    <div class="cerrar-modal">
      <img
        :src="cerrarModal"
        alt="cerrar modal"
        @click="$emit('ocultar-modal')"
      />
    </div>

    <div
      class="contenedor contenedor-formulario"
      :class="[modal.animar ? 'animar' : 'cerrar']"
    >
      <form class="nuevo-gasto" @submit.prevent="agregarGasto">
        <legend>{{ texto.encabezado }}</legend>

        <Alerta v-if="error">{{ error }}</Alerta>
        <div class="campo">
          <label for="nombre">Gasto:</label>
          <input
            type="text"
            id="nombre"
            placeholder="Escribe el Nombre del Gasto"
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
          />
        </div>

        <div class="campo">
          <label for="cantidad">Cantidad Gastada:</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Escribe la cantidad, Ej. 300"
            :value="cantidad"
            @input="$emit('update:cantidad', +$event.target.value)"
          />
        </div>

        <div class="campo">
          <label for="categoria">Categoria:</label>

          <select
            id="categoria"
            :value="categoria"
            @input="$emit('update:categoria', $event.target.value)"
          >
            <option value="">Selecciona una categoría</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="gastos">Gastos</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
        </div>

        <input type="submit" :value="texto.boton" />

        <button type="button" class="btn-eliminar" v-if="props.id">
          Eliminar Gasto
        </button>
      </form>
    </div>
  </div>
</template>

<style lang="css" scoped>
.modal {
  position: absolute;
  background-color: rgb(0 0 0 / 0.9);
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 200;
}
.cerrar-modal {
  position: absolute;
  right: 3rem;
  top: 3rem;
}
.cerrar-modal img {
  width: 2.5rem;
  cursor: pointer;
}
.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}
.contenedor-formulario.animar {
  opacity: 1;
}
.contenedor-formulario.cerrar {
  opacity: 0;
}
.nuevo-gasto {
  margin: 3rem auto 0 auto;
  display: grid;
  gap: 2rem;
  padding: 2.5rem;
}
@media (min-width: 768px) {
  .nuevo-gasto {
    padding: 5rem;
  }
}
.nuevo-gasto legend {
  text-align: center;
  color: var(--blanco);
  font-size: 3rem;
  font-weight: 700;
  padding-bottom: 1rem;
  border-bottom: 2px solid var(--anaranjado);
  text-transform: uppercase;
}
.campo {
  display: grid;
  gap: 2rem;
}
.nuevo-gasto label {
  color: var(--gris-claro);
  font-size: 2.5rem;
}
.nuevo-gasto input,
.nuevo-gasto select {
  background-color: var(--gris-claro);
  border-radius: 1rem;
  padding: 1rem;
  border: none;
  font-size: 2rem;
}
.nuevo-gasto input[type="submit"] {
  background-color: var(--anaranjado);
  color: var(--blanco);
  font-weight: 700;
  margin-top: 4rem;
  transition: background-color 300ms ease;
}
.nuevo-gasto input[type="submit"]:hover {
  background-color: var(--anaranjado-oscuro);
  cursor: pointer;
}

.btn-eliminar {
  background-color: #6f0909;
  width: 100%;
  color: var(--blanco);
  font-weight: 700;
  margin-top: 4rem;
  transition: background-color 300ms ease;
  border-radius: 1rem;
  padding: 0.6rem;
  border: none;
  font-size: 1.4rem;
}
.btn-eliminar:hover {
  background-color: #ab1616;
  cursor: pointer;
}
</style>
