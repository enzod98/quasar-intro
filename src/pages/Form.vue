<template>
    <q-page padding>
        <h4>Agregar Productos</h4>

        <q-form 
        @submit.prevent="procesarForm"
        @reset="limpiarForm"
        class="row q-col-gutter-md"
        ref="myForm"
        >

            <div class="col-12 col-sm-6">
                <q-input 
                label="Producto"
                v-model.trim="producto"
                lazy-rules
                :rules="[val => val && val.length > 0 || 'Por favor, escriba algo']"
                />
            </div>

            <div class="col-12 col-sm-6">
                <q-select 
                    label="Prioridad"
                    v-model="seleccion"
                    :options="opciones"
                    lazy-rules
                    :rules="[val => val && val.length > 0 || 'Seleccione una prioridad']"
                />
            </div>

            <div class="col-12">
                <q-toggle 
                label="Acepto los términos" 
                v-model="terminos"
                :rules="[val => val && val === true || 'Debe aceptar los términos para continuar']"
                />
            </div>

            <div class="col-12">
                <q-btn color="blue" label="Submit" type="submit" />
                <q-btn label="Reset" color="Primary" outline class="q-ml-sm" :ripple="false" type="reset" />
            </div>
        </q-form>

        <pintar-datos
            :productos="productos"
         />
    </q-page>
</template>

<script>
import {ref} from 'vue'
import { useQuasar } from 'quasar'
import PintarDatos from 'src/components/PintarDatos.vue';

export default {
  components: { PintarDatos },
    setup() {
        const $q = useQuasar();

        const myForm = ref(null);
        const producto = ref(null);
        const seleccion = ref(null);
        const terminos = ref(false);
        const opciones = ['máxima', 'moderada', 'mínima']

        const productos = ref([]);
        
        const procesarForm = () => {
            console.log('submited');
            if(!terminos.value) {
                $q.notify({
                color: 'red-5',
                textColor: 'white',
                icon: 'warning',
                message: 'Debe aceptar los términos!'
            })
            } else{
                $q.notify({
                color: 'green-4',
                textColor: 'white',
                icon: 'cloud_done',
                message: 'Se envió el formulario!'
                })


                //Procesar form
                productos.value.push({
                    producto: producto.value,
                    prioridad: seleccion.value
                })
                
                myForm.value.resetValidation();
                limpiarForm();
            }
        }

        const limpiarForm = () => {
            producto.value = null
            seleccion.value = null
            terminos.value = false
        }

        return { producto, seleccion, opciones, procesarForm, terminos, limpiarForm, myForm, productos}
    },
}
</script>