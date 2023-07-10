<script setup>
import HelloWorld from './components/HelloWorld.vue'
import ArrayBars from './components/ArrayBars.vue'
import { ref } from 'vue'

// generate random numbers for the array
function generateRandomArray(size) {
  width = 880 / size
  const array = []
  for (let i = 0; i < size; i++) {
    array.push(Math.floor(Math.random() * 95) + 1)
  }
  return array
}
let width = ref(880 / 10) // create a reactive width
let array = ref(generateRandomArray(10)) // create a reactive array
let speed = 50 // create a speed variable


// create web audio api context
var audioCtx = new(window.AudioContext || window.webkitAudioContext)();

function playNote(frequency, duration) {
  // create Oscillator node
  var oscillator = audioCtx.createOscillator();



  oscillator.type = 'square';
  oscillator.frequency.value = frequency; // value in hertz

    // set the volume very quiet
  const gainNode = audioCtx.createGain();
  gainNode.gain.value = 0.03;
  oscillator.connect(gainNode);
  gainNode.connect(audioCtx.destination);
  oscillator.start();

  setTimeout(
    function() {
      oscillator.stop();
    }, duration);
}


function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

function sort(array){
  // find out what sorting algorithm is selected
  let algo = document.getElementById("algo").value;
  // call the appropriate sorting algorithm
  if(algo == "bubble"){
    bubbleSort(array);
  }
  else if(algo == "insertion"){
    insertionSort(array);
  }
  else if(algo == "selection"){
    selectionSort(array);
  }
  else if(algo == "merge"){
    mergeSort(array);
  }
  else if(algo == "quick"){
    quickSort(array);
  }
  else if(algo == "heap"){
    heapSort(array);
  }
}

async function selectedBar(index){
  // change the color of the current bar
  let bars = document.getElementsByClassName("array-bar");
  bars[index].style.backgroundColor = "#ff0000";
  // console.log(index);
  await sleep(speed);
  bars[index].style.backgroundColor = "#ffffff";
}

async function bubbleSort(array){
  // Bubble sort is a sorting algorithm which compares two adjacent elements and swaps them if they are not in the intended order. this continues until the array is sorted. O(n^2)
  let size = array.length;
  for(let i = 0; i < size; i++){
    for(let j = 0; j < size - i - 1; j++){
      // change the color of the current bar
      playNote(array[j] * 10, 100);
      await selectedBar(j);
      if(array[j] > array[j + 1]){ // if the current element is greater than the next element, swap them
        let temp = array[j];
        array[j] = array[j + 1];
        array[j + 1] = temp;
      }
    }
  }
}

async function insertionSort(array){
  // Insertion sort is a sorting algorithm which inserts an element from unsorted array to its correct position in sorted array. O(n^2)
  let size = array.length;
  for(let i = 1; i < size; i++){
    let key = array[i];
    let j = i - 1;
    while(j >= 0 && array[j] > key){
      playNote(array[j] * 10, 100);
      await selectedBar(j);
      array[j + 1] = array[j];
      j--;
    }
    array[j + 1] = key;
  }

}

async function selectionSort(array){
  // Selection sort is a sorting algorithm which selects the smallest element from an unsorted list in each iteration and places that element at the beginning of the unsorted list. O(n^2)
  let size = array.length;
  for(let i = 0; i < size - 1; i++){
    let min = i;
    for(let j = i + 1; j < size; j++){
      playNote(array[j] * 10, 100);
      await selectedBar(j);
      if(array[j] < array[min]){
        min = j;
      }
    }
    let temp = array[min];
    array[min] = array[i];
    array[i] = temp;
  }
}

function merge(left, right){
  // helper function for merge sort
  let merged = [];
  let leftSize = left.length;
  let rightSize = right.length;
  let i = 0;
  let j = 0;
  while(i < leftSize && j < rightSize){
    if(left[i] <= right[j]){
      merged.push(left[i]);
      i++;
    }
    else{
      merged.push(right[j]);
      j++;
    }
  }
  while(i < leftSize){
    merged.push(left[i]);
    i++;
  }
  while(j < rightSize){
    merged.push(right[j]);
    j++;
  }
  console.log(merged);
  return merged;
}

function mergeSort(array){
  // Merge sort is a sorting algorithm which divides the input array into two halves, calls itself for the two halves, and then merges the two sorted halves. O(nlogn)
  let size = array.length;
  if(size < 2){
    return;
  }
  let mid = Math.floor(size / 2);
  let left = [];
  let right = [];
  for(let i = 0; i < mid; i++){
    left.push(array[i]);
  }
  for(let i = mid; i < size; i++){
    right.push(array[i]);
  }
  mergeSort(left);
  mergeSort(right);
  return merge(left, right, array);
}

async function quickSort(array){

}

async function heapSort(array){

}


</script>

<template>


  <!-- Forms to edit size of array and width of bar -->
  <div class="edit-box">
    <form>
      <label for="size">Size of Array:</label>
      <input type="number" id="size" name="size" min="1" max="100" v-model="array.length" @input="array = generateRandomArray(array.length)">
      
      <label for="width">Speed of Sorting (ms):</label>
      <input type="number" id="speed" name="speed" min="1" max="10000" v-model="speed">
      <!-- input for sorting algo -->
      <label for="algo">Sorting Algorithm:</label>
      <select name="algo" id="algo">
        <option value="bubble">Bubble Sort</option>
        <option value="insertion">Insertion Sort</option>
        <option value="selection">Selection Sort</option>
        <option value="merge">Merge Sort</option>
        <option value="quick">Quick Sort</option>
        <option value="heap">Heap Sort</option>
      </select>
    </form>
    <button @click="sort(array)">Sort!</button>
  </div>


  
  <!-- Create an ArrayBars object -->
  <ArrayBars :array="array" :width="width"></ArrayBars>
  
</template>

<style scoped>
.edit-box {
  height: 100%;
  padding: 1rem;
  background-color: #a10606;
  color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
}


</style>

