<template>
  <div class="container mt-4">
    <h2>Lista de personas</h2>

    <!-- Filtros -->
    <div class="mb-3">
      <input
        v-model="filtroNombre"
        type="text"
        class="form-control mb-2"
        placeholder="Buscar por nombre o apellido"
      />
      <input
        v-model="filtroDni"
        type="text"
        class="form-control"
        placeholder="Buscar por DNI"
      />
    </div>

    <!-- Alerta si filtros con menos de 3 caracteres -->
    <div v-if="mostrarAlerta" class="alert alert-warning" role="alert">
      Por favor, ingrese al menos 3 caracteres en los filtros.
    </div>

    <!-- Resultados filtrados -->
    <ul class="list-group">
      <li
        v-for="persona in personasFiltradas"
        :key="persona.dni"
        class="list-group-item"
      >
        {{ persona.nombre }} {{ persona.apellido }} - DNI: {{ persona.dni }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filtroNombre: '',
      filtroDni: '',
      personas: [
        { nombre: 'Ana', apellido: 'Gomez', dni: '12345678' },
        { nombre: 'Juan', apellido: 'Perez', dni: '87654321' },
        { nombre: 'Luis', apellido: 'Martinez', dni: '11223344' },
        // podés agregar más personas acá
      ],
    }
  },
  computed: {
    mostrarAlerta() {
      return (
        (this.filtroNombre && this.filtroNombre.length > 0 && this.filtroNombre.length < 3) ||
        (this.filtroDni && this.filtroDni.length > 0 && this.filtroDni.length < 3)
      )
    },
    personasFiltradas() {
      if (this.mostrarAlerta) return []

      if (!this.filtroNombre && !this.filtroDni) return this.personas

      return this.personas.filter((p) => {
        const nombreCompleto = (p.nombre + ' ' + p.apellido).toLowerCase()
        const filtroNom = this.filtroNombre.toLowerCase()

        const coincideNombre =
          !this.filtroNombre || nombreCompleto.includes(filtroNom)

        const coincideDni =
          !this.filtroDni || p.dni.includes(this.filtroDni)

        return coincideNombre && coincideDni
      })
    },
  },
}
</script>
