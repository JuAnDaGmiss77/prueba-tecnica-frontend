<script>
import useVuelidate from '@vuelidate/core'
import { required, email, minLength,integer,not } from '@vuelidate/validators'


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
        nombreCompleto: '',
        email: '',
        numCelular: '',
        departamento: '',
        ciudad:'',
        acceptTerminos:''
      },
      
    }
  },

  validations() {
    return {
      form: {
        nombreCompleto: { 
          required,min: minLength(5),name_validation: {
            $validator: validName,
            $message: 'Ingrese un nombre valido, no deje espacios ni al inicio ni final'
          } 
        },
        modelo:{required},
        email: { required, email },
        numCelular: {required, min: minLength(10), integer},
        //departamento: {required},
        //ciudad:{required}
      },
      
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
                        <input v-model="v$.form.nombreCompleto.$model" type="text" class="form-control" />
                        <div v-for="(error, index) of v$.form.nombreCompleto.$errors" :key="index">
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
                        <input v-model="v$.form.numCelular.$model" type="text" class="form-control" />
                        <div v-for="(error, index) of v$.form.numCelular.$errors" :key="index">
                            <span>{{ error.$message }}</span>
                        </div>
                    </div>
                    <div class="mb-3 row">
                        <div class="col-6">
                            <label class="form-label">Departamento</label>
                            <select class="form-select">
                                <option value>prueba</option>
                                <option value>prueba</option>
                            </select>
                        </div>

                        <div class="col-6">
                            <label class="form-label">Ciudad</label>
                            <select class="form-select">
                                <option value>prueba</option>
                                <option value>prueba</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-check mb-3">
                        <input v-model="form.acceptTerminos" class="form-check-input" type="checkbox" value="form.acceptTerminos" id="flexCheckDefault" />
                        <label class="form-check-label" for="flexCheckDefault">Acepto la politica de <a href="">Tratamiento de datos personales</a></label>
                        {{form.acceptTerminos}}
                    </div>

                    <button :disabled="v$.form.$invalid" type="submit" class="btn btn-dark">Enviar datos</button>
                    {{v$.form.$invalid}}
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