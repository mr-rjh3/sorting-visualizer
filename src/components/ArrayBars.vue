<script setup>

// define props defines the props that are passed to the component
defineProps({ 
    // an array of numbers is passed to this component
    array: Array,
    width: Number,
})
</script>

<template>
    <div class="array-container">
        <!-- v-for is a directive that loops through the array and creates a div for each number in the array -->
        <!-- :key="num" is used to tell vue that each div is unique -->
        <!-- :style="{height: num + 'px', width: size + 'px'}" is used to set the height and width of each div -->
        <div class="array-bar" v-for="(num, index) in array" :key="index" :style="{height: num + '%', width: width + 'px'}"></div>
    </div>
</template>

<style scoped>
.array-container {
    display: flex;
    justify-content: center;
    align-items: flex-end; /* aligns the bottom of the divs */
    height: 100%;
    width: 100%;
    background-color: #000000;
    overflow: auto;
}
.array-bar {
    background-color: #ffffff;
    margin: 0 0.5px;
}
</style>