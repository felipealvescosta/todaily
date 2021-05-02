<template>
    <div class="card">
        <slot/>
    </div>
</template>

<script>
export default {
    name: 'Card',
}
</script>

<style scoped>
.card {
    background: var(--color-white);
    padding: 1.2rem;
    border-radius: 0.4rem;
    box-shadow: 0 1px rgba(33, 33, 33, 0,8);
    margin-bottom: 1.2rem;
    font-size: 1.8rem;
    cursor: pointer;
}
</style>