<script setup>
import { formatearDinero, formatearFecha } from "../helpers/index";
import IconoAhorro from "../assets/img/icono_ahorro.svg";
import IconoCasa from "../assets/img/icono_casa.svg";
import IconoComida from "../assets/img/icono_comida.svg";
import IconoGastos from "../assets/img/icono_gastos.svg";
import IconoHobbies from "../assets/img/icono_hobbies.svg";
import IconoSalud from "../assets/img/icono_salud.svg";
import IconoSuscripciones from "../assets/img/icono_suscripciones.svg";
import IconoRopa from "../assets/img/icono_ropa.svg";
import IconoDeudas from "../assets/img/icono_deudas.svg";
import IconoImpuestos from "../assets/img/icono_impuestos.svg";
import IconoTransporte from "../assets/img/icono_transporte.svg";
import IconoMascotas from "../assets/img/icono_mascotas.svg";
import IconoViajes from "../assets/img/icono_viajes.svg";
import IconoEstetica from "../assets/img/icono_cuidadopersonal.svg";
import IconoAccesorios from "../assets/img/icono_accesorios.svg";
import IconoEventos from "../assets/img/icono_eventos.svg";

const diccionarioIconos = {
  ahorro: IconoAhorro,
  comida: IconoComida,
  casa: IconoCasa,
  gastos: IconoGastos,
  hobbies: IconoHobbies,
  salud: IconoSalud,
  suscripciones: IconoSuscripciones,
  ropa: IconoRopa,
  impuestos: IconoImpuestos,
  deudas: IconoDeudas,
  transporte: IconoTransporte,
  viajes: IconoViajes,
  estetica: IconoEstetica,
  mascotas: IconoMascotas,
  accesorios: IconoAccesorios,
  eventos: IconoEventos,
};

defineEmits(["seleccionar-gasto"]);

const props = defineProps({
  gasto: {
    type: Object,
    required: true,
  },
});
</script>
<template>
  <div class="gasto sombra" @click="$emit('seleccionar-gasto', gasto.id)">
    <div class="contenido">
      <img :src="diccionarioIconos[gasto.categoria]" alt="" class="icono" />
      <div class="detalles">
        <p class="categoria">{{ gasto.categoria }}</p>
        <p class="nombre">{{ gasto.nombre }}</p>
        <p class="fecha">
          Fecha: <span>{{ formatearFecha(gasto.fecha) }}</span>
        </p>
      </div>
    </div>
    <p class="cantidad">{{ formatearDinero(gasto.cantidad) }}</p>
  </div>
</template>

<style lang="css" scoped>
.gasto {
  display: flex;
  flex-direction: column;
  margin-bottom: 2rem;
  cursor: pointer;
}
.contenido {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}
@media (min-width: 768px) {
  .gasto {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  .contenido {
    flex-direction: row;
    align-items: center;
    gap: 2rem;
  }
}
.icono {
  width: 8rem;
}
.detalles p {
  margin: 0 0 1rem 0;
}
.categoria {
  color: var(--gris-claro);
  font-size: 1.2rem;
  text-transform: uppercase;
  font-weight: 900;
}
.nombre {
  color: var(--anaranjado);
  font-size: 2.4rem;
  font-weight: 700;
  text-transform: capitalize;
}
.fecha {
  color: var(--gris-oscuro);
  font-size: 1.6rem;
  font-weight: 900;
}
.fecha span {
  font-weight: 400;
}
.cantidad {
  font-size: 3rem;
  font-weight: 900;
  margin: 0;
  color: var(--verde);
}
</style>
