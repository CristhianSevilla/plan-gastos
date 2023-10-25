<script setup>
import { computed } from "vue";
import CircleProgress from "vue3-circle-progress-bar";
import "vue3-circle-progress-bar/dist/circle-progress-bar.css";
import { formatearDinero } from "../helpers";

const props = defineProps({
  presupuesto: {
    type: Number,
    required: true,
  },
  disponible: {
    type: Number,
    required: true,
  },
  gastado: {
    type: Number,
    required: true,
  },
});

const porcentaje = computed(() => {
  return parseInt(
    ((props.presupuesto - props.disponible) / props.presupuesto) * 100
  );
});
</script>
<template>
  <div class="dos-columnas">
    <div class="contenedor-grafico">
      <p class="porcentaje">{{ porcentaje }}%</p>

      <CircleProgress
        :is-gradient="true"
        :gradient="{
          angle: 90,
          startColor: '#e23b04',
          stopColor: '#f5ab00',
        }"
        :percent="porcentaje"
        :size="200"
        :border-width="20"
        :border-bg-width="20"
      />
    </div>
    <div class="contenedor-presupuesto">
      <button class="reset-app">Resetear App</button>
      <p>
        <span>Presupuesto:</span>
        {{ formatearDinero(presupuesto) }}
      </p>
      <p>
        <span>Disponible:</span>
        {{ formatearDinero(disponible) }}
      </p>
      <p>
        <span>Gastado:</span>
        {{ formatearDinero(gastado) }}
      </p>
    </div>
  </div>
</template>

<style lang="css" scoped>
.dos-columnas {
  display: flex;
  flex-direction: column;
}
.dos-columnas > :first-child {
  margin-bottom: 4rem;
}
@media (min-width: 768px) {
  .dos-columnas {
    flex-direction: row;
    gap: 4rem;
    align-items: center;
  }
  .dos-columnas > :first-child {
    margin-bottom: 0rem;
  }
}
.reset-app {
  background-color: var(--anaranjado);
  border: none;
  padding: 1rem;
  width: 100%;
  color: var(--blanco);
  font-weight: 900;
  text-transform: uppercase;
  border-radius: 1rem;
  transition-property: background-color;
  transition-duration: 300ms;
}
.reset-app:hover {
  background-color: var(--anaranjado-oscuro);
  cursor: pointer;
}
.contenedor-grafico {
  margin: 0 auto;
  position: relative;
}
.porcentaje {
  position: absolute;
  margin: auto;
  top: calc(50% - 1.5rem);
  left: 0;
  right: 0;
  text-align: center;
  z-index: 100;
  font-size: 3rem;
  font-weight: 900;
  color: var(--anaranjado);
}
.contenedor-presupuesto {
  width: 100%;
}
.contenedor-presupuesto p {
  font-size: 2.4rem;
  color: var(--gris);
}
.contenedor-presupuesto span {
  font-weight: 900;
  color: var(--anaranjado);
}
</style>
