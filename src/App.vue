<script setup>
    import {ref, reactive, watch, onMounted} from 'vue'
    import { uid } from 'uid'
    import Header from './components/Header.vue'
    import Formulario from './components/Formulario.vue'
    import Paciente from './components/Paciente.vue'

    

    const pacientes = ref([])

    const paciente = reactive({
        id : null,
        nombre: '',
        propietario: '',
        email: '',
        alta: '',
        sintomas: ''

    })
    watch(paciente, () =>{
        guardarLocalStorage()
    },{
        deep: true
    })
    

    const guardarLocalStorage = ()=>{
        localStorage.setItem('pacientes', JSON.stringify(pacientes.value))

    }

    onMounted(()=>{
        const pacientesLS = JSON.parse(localStorage.getItem('pacientes'))
        if(pacientesLS){
            pacientes.value = pacientesLS
        }
    })

    const guardarPaciente = () =>{
        if(paciente.id){
            const index = pacientes.value.findIndex(paciente => paciente.id === paciente.id)
            pacientes.value[index] = {...paciente}
        }else{
            pacientes.value.push({
            ...paciente,
            id: uid()
        })
        }


        //Reiniciar el objeto
        paciente.nombre = ''
        paciente.propietario = ''
        paciente.email = ''
        paciente.alta = ''
        paciente.sintomas = ''
        //otra forma
        // Object.assign(paciente,{
        //     nombre: '',
        //     propietario: '',
        //     email: '',
        //     alta: '',
        //     sintomas: ''
        // })
    }
    const eliminarPaciente = (id) =>{
        pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
    }

    const editarPaciente = (id) =>{
        const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
        Object.assign(paciente, pacienteEditar)
    }
    



</script>


<template>
    <div class="container mx-auto mt-20">

        <Header />

        <div class="mt-12 md:flex">

            <Formulario 
                v-model:nombre="paciente.nombre"
                v-model:propietario="paciente.propietario"
                v-model:email="paciente.email"
                v-model:sintomas="paciente.sintomas"
                v-model:alta="paciente.alta"
                @guardar-paciente="guardarPaciente"
                :id="paciente.id"
            
            />

            <div class="md:w-1/2 md:h-screen overflow-y-scroll">
                <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
            
                <div v-if="pacientes.length > 0" >
                    <p class="text-lg mt-5 text-center mb-10">
                        Informacion de
                        <span class="text-indigo-400 font-bold ">Pacientes </span>

                    </p>
                    <Paciente
                        v-for="paciente in pacientes"
                        :paciente="paciente"
                        @eliminar-paciente="eliminarPaciente"
                        @editar-paciente="editarPaciente"
                    
                    />

                </div>

                <p v-else class="text-2xl mt-20 text-center " >No hay pacientes</p>
            
            </div>

        </div>
    </div>

</template>
