<script setup>
    import { computed, defineProps } from 'vue'
    const props = defineProps({
        alerta: {
            type: Object,
            required: true
        }

    })

    const isError = computed(() => props.alerta.tipo === 'error')

</script>


<template>
    <div
        :class="[isError ? 'bg-red-500' : 'bg-green-500']"
        class="text-white p-5 my-3 rounded-md text-center mb-3 uppercase font-bold"
    
    >
        {{alerta.mensaje}}
    </div>
</template>
