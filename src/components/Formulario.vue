<script setup>
    import {reactive, computed } from 'vue'
    import Alerta from './Alerta.vue';

    const alerta = reactive({
        tipo: '',
        mensaje: '',
        
    })

    const props = defineProps({
        id:{
            type: [String , null],
            required: true,
            
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    })

    const editando = computed(() =>{
        return props.id !== null
    })

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email'
    , 'update:alta', 'update:sintomas', 'guardarPaciente', 'editarPaciente', 'eliminarPaciente'
    ])

    
    const validar = () => {
        if(Object.values(props).includes('')){
            alerta.mensaje= 'Todos los campos son obligatorios'
            alerta.tipo = 'error'
            return
        }
        emit('guardarPaciente')
        alerta.mensaje= 'Paciente almacenado correctamente'
        alerta.tipo = 'success'

        setTimeout(()=>{
            Object.assign(alerta,{
                mensaje: '',
                tipo: ''
            })
        }, 5000)

    }

</script>



<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center   ">Seguimiento pacientes</h2>
    


        <Alerta 
            v-if="alerta.mensaje" 
            :alerta="alerta"
        />

        <form  
            class="bg-white shadow-md rounded-lg py-10 px-5 mt-10 "
            @submit.prevent ="validar"
        >
            <div class="mb-5 ">

                
                <label 
                    for="mascota"
                    class="block text-gray-700 uppercase text-sm font-bold mb-2"
                >
                    Nombre Mascota
                </label>

                <input 
                    type="text"
                    id="mascota" 
                    placeholder="Nombre de la Mascota"
                    class="border-2 border-gray-200 rounded-lg px-3 py-3 w-full focus:outline-none focus:border-indigo-400 mt-2"
                    :value="nombre"
                    @input="$emit('update:nombre', $event.target.value) "
                />

            </div>

            <div class="mb-5 ">
                <label 
                    for="propietario"
                    class="block text-gray-700 uppercase text-sm font-bold mb-2"
                >
                    Nombre Propietario
                </label>

                <input 
                    type="text"
                    id="propietario"
                    placeholder="Nombre del Propietario"
                    class="border-2 border-gray-200 rounded-lg px-3 py-3 w-full focus:outline-none focus:border-indigo-400 mt-2"   
                    :value="propietario"
                    @input="$emit('update:propietario', $event.target.value) "

                
                />

            </div>
            <div class="mb-5 ">
                <label 
                    for="email"
                    class="block text-gray-700 uppercase text-sm font-bold mb-2"
                >
                    Email
                </label>

                <input 
                    type="text"
                    id="email"
                    placeholder="Email del Propietario"
                    class="border-2 border-gray-200 rounded-lg px-3 py-3 w-full focus:outline-none focus:border-indigo-400 mt-2"   
                    :value="email"
                    @input="$emit('update:email', $event.target.value) "

                />

            </div>
            <div class="mb-5 ">
                <label 
                    for="Alta"
                    class="block text-gray-700 uppercase text-sm font-bold mb-2"
                >
                    Alta
                </label>

                <input 
                    type="date"
                    id="Alta" 
                    class="border-2 border-gray-200 rounded-lg px-3 py-3 w-full focus:outline-none focus:border-indigo-400 mt-2"   
                    :value="alta"
                    @input="$emit('update:alta', $event.target.value) "

                />

            </div>
            <div class="mb-5 ">
                <label 
                    for="sintomas"
                    class="block text-gray-700 uppercase text-sm font-bold mb-2"
                >
                    Síntomas
                </label>

                <textarea 
                    id="sintomas" 
                    placeholder="Describe los síntomas"
                    class="h-40 border-2 border-gray-200 rounded-lg px-3 py-3 w-full focus:outline-none focus:border-indigo-400 mt-2"   
                    :value="sintomas"
                    @input="$emit('update:sintomas', $event.target.value) "

                />

            </div>

            <input 
                type="submit"
                class="font-bold uppercase text-center w-full rounded-lg bg-indigo-400 text-white p-3
                hover:text-gray-100 hover:bg-indigo-600 transition-colors"
                :value="[editando ? 'Editar Paciente' : 'Registrar Paciente']"


            />

        </form>

    </div>
</template>

