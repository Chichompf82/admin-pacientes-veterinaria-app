<template lang="html">
  
  <section class="table-responsive">
    <h2 class="text-center my-2 text-success">Citas Pendientes</h2>
    <table class="table table-dark">
        <tr>
            <th>Id</th>
            <th>Nombre de Mascota</th>
            <th>Nombre de Dueño</th>
            <th>Fecha atención</th>
            <th>Email</th>
            <th>Síntomas</th>
            <th></th>
        </tr>
        <tr v-for="cita in citasPendientes" :key="cita.id">
            <td>{{ cita.id }}</td>
            <td>{{ cita.nombre }}</td>
            <td>{{ cita.nombre_duenio }}</td>
            <td>{{ cita.fecha_hora_atencion }}</td>
            <td>{{ cita.email }}</td>
            <td>{{ cita.sintomas }}</td>
            <td><button @click="pasarAHistorial(cita)">Pasar a Atendido</button></td>
        </tr>
    </table>

    <h4 v-if="!citasPendientes.length" class="alert alert-info">
        <span>No hay citas pendientes</span>
    </h4>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'appointments-view',
    props: [],
    mounted () {
      this.getCitas();
    },
    data () {
      return {
        url: "https://62b25de3c7e53744afcb7292.mockapi.io/admin-vet/citas/",
        citasPendientes: []
      }
    },
    methods: {
      async getCitas() {
        try {
          let { data: citas } = await this.axios.get(this.url);
          console.log("AXIOS POST Citas", citas);
          let citasPendientes = citas.filter(cita => !cita.atendido);
          this.citasPendientes = citasPendientes;
        } catch (error) {
          console.error("Error en getCitas()", error.message);
        }
      },
      async pasarAHistorial(cita) {
        let citaUpdate = {
          ...cita,
          atendido: true
        }
        try {
        let { data: citaActualizada } = await this.axios.put(this.url+cita.id, citaUpdate, {'content-type' : 'application/json'})
          console.log("AXIOS PUT Citas", citaActualizada);
          this.getCitas()
        } catch (error) {
          console.error("Error en pasarAHistorial()", error.message);
        }

      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
tr td {
  color: white;
}
</style>
