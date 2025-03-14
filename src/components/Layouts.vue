<template>
    <div class="container">
        <header v-if="$slots.header">
            <slot name="header"></slot>
        </header>
        <main>
            <slot name="main"></slot>
        </main>
    </div>
</template>


