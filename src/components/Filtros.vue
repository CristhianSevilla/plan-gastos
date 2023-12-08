<script setup>
defineProps(["filtro"]);
defineEmits(["update:filtro"]);
</script>

<template>
  <div class="filtros sombra contenedor">
    <form>
      <div class="campo">
        <label for="filtro">Filtrar Gastos</label>
        <select
          id="filtro"
          :value="filtro"
          @input="$emit('update:filtro', $event.target.value)"
        >
          <option value="">Todas las categorías</option>
          <option value="accesorios">Accesorios</option>
          <option value="ahorro">Ahorro</option>
          <option value="casa">Casa</option>
          <option value="comida">Comida</option>
          <option value="deudas">Deudas</option>
          <option value="estetica">Estética</option>
          <option value="eventos">Eventos</option>
          <option value="gastos">Gastos</option>
          <option value="hobbies">Hobbies</option>
          <option value="impuestos">Impuestos</option>
          <option value="mascotas">Mascotas</option>
          <option value="ropa">Ropa</option>
          <option value="salud">Salud</option>
          <option value="suscripciones">Suscripciones</option>
          <option value="transporte">Transporte</option>
          <option value="viajes">Viajes</option>
        </select>
      </div>
    </form>
  </div>
</template>

<style lang="css" scoped>
.filtros {
  margin-top: 22rem;
}

.filtros .campo {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2rem;
}
@media (min-width: 768px) {
  .filtros .campo {
    flex-direction: row;
    justify-content: space-between;
  }
}
.filtros .campo select {
  border: var(--anaranjado) solid 3px;
  border-radius: 1rem;
  padding: 1rem;
  background-color: var(--negro);
  flex: 1;
  width: 100%;
  color: var(--gris-claro);
}
.filtros label {
  color: var(--anaranjado);
  font-size: 2.5rem;
  font-weight: 900;
}
</style>
