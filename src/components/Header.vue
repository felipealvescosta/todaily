<template>
    <header class="header">
        <img 
            class="logo"
            src="../assets/todaily.png" 
            alt="ToDaily"
        />
    </header>
</template>
<script>
export default {
}
</script>
<style scoped>
.header {
    width: 100%;
    height: 10rem;
    background: var(--color-primary);    
    display: flex;
    justify-content: center;
    align-items: center;
}

.logo {
    width: 24rem;
}
</style>