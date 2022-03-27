<script>
import useVuelidate from '@vuelidate/core'
import { required, email, minLength,integer,maxLength} from '@vuelidate/validators'
import Swal from 'sweetalert2'

import 'sweetalert2/dist/sweetalert2.min.css';
import axios from 'axios'


export function validName(name) {
  let validNamePattern = new RegExp("^[a-zA-Z]+(?:[-'\\s][a-zA-Z]+)*$");
  if (validNamePattern.test(name)){
    return true;
  }
  return false;
}


export default {

  setup () {
    return { v$: useVuelidate() }
  },

  data() {
    return {
      form: {
        modelo: '',
        nombre_completo: '',
        email: '',
        numero_celular: 0,
        departamento: '',
        ciudad:'',
        acceptTerminos:''
      },
      departamentos: [
          {
          id:1,
          nombre:"Antioquia"
          },
          {
          id:3,
          nombre:"Bogota DC"
          },

        ],
        ciudades: {
            nombre:''
        }
           
      
    }
  },

  validations() {
    return {
      form: {
        nombre_completo: { 
          required,min: minLength(5),name_validation: {
            $validator: validName,
            $message: 'Ingrese un nombre valido, no deje espacios ni al inicio ni final'
          } 
        },
        modelo:{required},
        email: { required, email },
        numero_celular: {required, min: minLength(10), max: maxLength(10) , integer},
        departamento: {required},
        ciudad:{required}
      },
      
    }
  },
  methods: {
      actualizarCiudades(){
          if(this.form.departamento == 'Antioquia'){
              this.ciudades.nombre = 'Medellin'
          }
          if (this.form.departamento == 'Bogota DC') {
              this.ciudades.nombre = 'Bogota'
          }
      },
      async enviarCotizacion(){
          if (this.form.acceptTerminos == false) {
             return Swal.fire({
                icon: 'warning',
                title: 'Oops...',
                text: 'Necesitas aceptar los tratamientos de datos personales',
            })
          }
          try {
              const response =  await axios.post('http://127.0.0.1:8000/api/quote',this.form);
              if (response.data.success == true) {
                return Swal.fire({
                    icon: 'success',
                    title: response.data.message,
                })
              }
              if (response.data.success == false) {
                return Swal.fire({
                    icon: 'warning',
                    title: response.data.message,
                    text: 'intentelo despues'
                })
              }
          } catch (error) {
              return Swal.fire({
                    icon: 'error',
                    title: 'Ha ocurrido un error en el servidor',
                })
          }
      }
  },
}
</script>

<template>
    <div class="body">
        <div class="container-form sign-up row">
            <div class="col-lg-6"></div>
            <div class="col-lg-6 d-flex justify-content-md-end justify-content-sm-between">
                <form @submit.prevent="onSubmit" class="formulario">
                    <h1>¡Cotiza la tuya!</h1>
                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">Modelos</label>
                        <select v-model="v$.form.modelo.$model" class="form-select">
                            <option disabled value="">Seleccione un modelo</option>
                            <option value="Toyota">Toyota</option>
                            <option value="Mercedes">Mercedes</option>
                            <option value="Chevrolet">Chevrolet</option>                           
                        </select>
                        <div v-for="(error, index) of v$.form.modelo.$errors" :key="index">
                            <span>{{ error.$message }}</span>
                        </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Nombre completo</label>
                        <input v-model="v$.form.nombre_completo.$model" type="text" class="form-control" />
                        <div v-for="(error, index) of v$.form.nombre_completo.$errors" :key="index">
                            <span>{{ error.$message }}</span>
                        </div>
                    </div>

                    <div class="mb-3">
                        <label class="form-label">Email</label>
                        <input v-model="v$.form.email.$model" type="text" class="form-control" />
                        <div v-for="(error, index) of v$.form.email.$errors" :key="index">
                            <span>{{ error.$message }}</span>
                        </div>
                    </div>

                    <div class="mb-3">
                        <label class="form-label">Numero celular</label>
                        <input v-model="v$.form.numero_celular.$model" type="number" class="form-control" />
                        <div v-for="(error, index) of v$.form.numero_celular.$errors" :key="index">
                            <span>{{ error.$message }}</span>
                        </div>
                    </div>
                    <div class="mb-3 row">
                        <div class="col-6">
                            <label class="form-label">Departamento</label>
                            <select @change="actualizarCiudades()" v-model="v$.form.departamento.$model"   class="form-select">
                                <option disabled value="">Seleccione un departamento</option>
                                <option v-for="departamento in departamentos" :key="departamento.id" :value="departamento.nombre">{{departamento.nombre}}</option>
                            </select>
                        </div>
                        <div class="col-6">
                            <label class="form-label">Ciudad</label>
                            <select v-model="v$.form.ciudad.$model" v-if="form.departamento != ''" class="form-select">
                                <option disabled value="">Seleccione una ciudad</option>
                                <option :value="ciudades.nombre">{{ciudades.nombre}}</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-check mb-3">
                        <input v-model="form.acceptTerminos" class="form-check-input" type="checkbox" value="form.acceptTerminos" id="flexCheckDefault" />
                        <label class="form-check-label" for="flexCheckDefault">Acepto la politica de <a target="_blank" href="https://cdnprocessoft.s3.amazonaws.com/SITIOS/XuQlxMLqjS_vGRUP1rcXyNlDKh3dwzcsokrQU-C0s2w/files/PTDP%20-%20PROCESSOFT%20S%20A%20S.pdf">Tratamiento de datos personales</a></label>
                        
                    </div>

                    <button @click="enviarCotizacion()" :disabled="v$.form.$invalid" type="submit" class="btn btn-dark">Enviar datos</button>
                    
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped>

.body {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-image: url("../assets/img/Img_car.png");
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
}

.container-form {
    width: 100%;
    height: 100%;
    transition: all 0.5s ease-out;
    padding-bottom: 10px;
    margin-top: 10px;
}

.formulario {
    width: 450px;
    padding: 1rem;
    margin: 2rem;
    background-color: rgb(240, 240, 240);
    border-radius: 7px;
}
@media (max-width: 768px) {
    .formulario {
    width: 100vh;
    height: 100%;
    padding: 1rem;
    margin: 0.5rem;
    background-color: rgb(240, 240, 240);
    border-radius: 7px;
}
}

h1 {
    margin-top: 10px;
    margin-bottom: 10px;
    font-size: 1.7rem;
    color: black;
}
.btn-dark {
    background-color: black;
    border-radius: 50px;
    width: 150px;
}
span{
    color: red;
}
</style>