<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-10 col-md-12 col-sm-12">
        <div class="card">
          <div class="card-body">
            <h2 class="card-title">Usuarios</h2>

            <table class="table">
              <thead>
                <tr>
                  <th>Nombre de usuario</th>
                  <th>Fecha de nacimiento</th>
                  <th>Edad</th>
                  <th>Dirección</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="usuario in usuarios" :key="usuario.id">
                  <td>{{ usuario.name }}</td>
                  <td>{{ usuario.fecha_nacimiento }}</td>
                  <td>{{ calcularEdad(usuario.fecha_nacimiento) }}</td>
                  <td>{{ concatenarDireccion(usuario) }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      usuarios: [],
    };
  },
  methods: {
    obtenerUsuarios() {
      axios.get('http://127.0.0.1:8000/consumo/users')
        .then(response => {
          this.usuarios = response.data;
          console.log(this.usuarios)
        })
        .catch(error => {
          console.error(error);
        });
    },
    calcularEdad(fechaNacimiento) {
      let today = new Date()
      let fechaNac = new Date(fechaNacimiento)
      let edad = today.getFullYear() - fechaNac.getFullYear()
      let diferenciaMeses = today.getMonth() - fechaNac.getMonth()
      if (
        diferenciaMeses < 0 ||
        (diferenciaMeses === 0 && today.getDate() < fechaNac.getDate())
      ) {
        edad--
      }
      return edad
    },
    concatenarDireccion(usuario) {
      return usuario.domicilio+' - '+usuario.numero_exterior+' - '+usuario.colonia+' - '+usuario.cp+' - '+usuario.ciudad
    },
  },
  created() {
    this.obtenerUsuarios();
  },
};
</script>

<style>
/* Estilos CSS adicionales si los necesitas */
</style>
