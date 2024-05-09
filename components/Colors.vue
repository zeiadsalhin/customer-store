<script setup>

</script>
<template>
    <div>
        <h1>Available Colors:</h1>
        <v-chip-group filter>
            <v-chip><v-icon size="x-small">mdi-circle</v-icon> White</v-chip>

            <v-chip class="text-green "><v-icon size="x-small">mdi-circle</v-icon> Green</v-chip>

            <v-chip class="text-blue"><v-icon size="x-small">mdi-circle</v-icon>Blue</v-chip>
            <v-chip class="text-grey"><v-icon size="x-small">mdi-circle</v-icon>Grey</v-chip>
        </v-chip-group>
    </div>
</template>