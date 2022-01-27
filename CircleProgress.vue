<template>
    <svg width="30" height="30" viewBox="0 0 120 120">
        <circle cx="60" cy="60" r="54" fill="none" stroke="#e6e6e6" stroke-width="12" />
        <circle :style="{'stroke-dashoffset': `calc(100 - ${percent})`}" class="percent fifty" cx="60" cy="60" r="54" fill="none" stroke="#3b82f6" stroke-width="12" pathLength="100" />
    </svg>
</template>

<script>
export default {
   props: {
       percent: {
           type: Number ,
           default: 10
       }
   }
}
</script>

<style scoped>

svg {
    transform: rotate(-90deg);
    top: calc(50px - 15px);
    left: calc(50px - 15px);
}

.percent {
    stroke-dasharray: 100;
}


</style>
