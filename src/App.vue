<script setup>
import ArrayBars from './components/ArrayBars.vue'
import { ref } from 'vue'

// generate random numbers for the array
function generateRandomArray(size) {
  width = Math.round(880 / size)
  // console.log(width)
  const array = []
  for (let i = 0; i < size; i++) {
    array.push(Math.floor(Math.random() * 95) + 1)
  }
  return array
}
let width = ref(880 / 10) // create a reactive width
let array = ref(generateRandomArray(10)) // create a reactive array
let speed = ref(50) // create a speed variable
let playSounds = ref(true) // create a playSounds variable

let algo = "Bubble Sort" // create a reactive algo variable
let msg = "Bubble sort is a sorting algorithm which compares two adjacent elements and swaps them if they are not in the intended order. this continues until the array is sorted."
let bigO = "O(n^2)"


// create web audio api context
var audioCtx = new(window.AudioContext || window.webkitAudioContext)();

function playNote(frequency, duration) {
  if(!playSounds.value){
    return;
  }
  // create Oscillator node
  var oscillator = audioCtx.createOscillator();

  oscillator.type = 'square';
  oscillator.frequency.value = frequency; // value in hertz

    // set the volume very quiet
  const gainNode = audioCtx.createGain();
  gainNode.gain.value = 0.02;
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

function changeDesctiption(){
  let algorithm = document.getElementById("algo").value;
  let header = document.getElementById("description-header");
  let message = document.getElementById("description-message");
  let bigO = document.getElementById("description-bigo");
  // console.log(header);
  if(algorithm == "bubble"){
    header.innerText = "Bubble Sort";
    message.innerText = "Bubble sort is a sorting algorithm which compares two adjacent elements and swaps them if they are not in the intended order. this continues until the array is sorted.";
    bigO.innerText = "O(n^2)";
  }
  else if(algorithm == "insertion"){
    header.innerText = "Insertion Sort";
    message.innerText = "Insertion sort is a sorting algorithm that places an unsorted element at its suitable place in a new array each iteration. It is less efficient on large lists than more advanced algorithms such as quicksort, heapsort, or merge sort.";
    bigO.innerText = "O(n^2)";
  }else if(algorithm == "selection"){
    header.innerText = "Selection Sort";
    message.innerText = "Selection sort is a sorting algorithm that selects the smallest element from an unsorted list in each iteration and places that element at the beginning of the unsorted list.";
    bigO.innerText = "O(n^2)";
  }
  else if(algorithm == "merge"){
    header.innerText = "Merge Sort";
    message.innerText = "Merge sort is a sorting technique based on divide and conquer technique. With worst-case time complexity being Ο(n log n), it is one of the most respected algorithms.";
    bigO.innerText = "O(n log n)";
  }
  else if(algorithm == "quick"){
    header.innerText = "Quick Sort";
    message.innerText = "Quick sort is a highly efficient sorting algorithm and is based on partitioning an array of data into smaller arrays. A large array is partitioned into two arrays one of which holds values smaller than a specified pivot value and the other holds values larger (in this implementation the pivot is the last value in each array). This continues on each sub-array until the array is sorted.";
    bigO.innerText = "O(n log n)";
  }
  else if(algorithm == "heap"){
    header.innerText = "Heap Sort";
    message.innerText = "Heap sort is a comparison-based sorting technique based on Binary Heap data structure. It is similar to selection sort where we first find the maximum element and place the maximum element at the end. We repeat the same process for the remaining elements.";
    bigO.innerText = "O(n log n)";
  }
  // console.log(algo);
}

async function sort(array){
  // prevent use of form while sorting
  document.getElementById("size").disabled = true;
  document.getElementById("algo").disabled = true;
  document.getElementById("shuffle").disabled = true;
  document.getElementById("sort").disabled = true;
  // find out what sorting algorithm is selected
  let algorithm = document.getElementById("algo").value;
  // call the appropriate sorting algorithm
  if(algorithm == "bubble"){
    await bubbleSort(array);
  }
  else if(algorithm == "insertion"){
    await insertionSort(array);
  }
  else if(algorithm == "selection"){
    await selectionSort(array);
  }
  else if(algorithm == "merge"){
    await mergeSort(array);
  }
  else if(algorithm == "quick"){
    await quickSort(array);
  }
  else if(algorithm == "heap"){
    await heapSort(array);
  }
  // re-enable form
  document.getElementById("size").disabled = false;
  document.getElementById("algo").disabled = false;
  document.getElementById("shuffle").disabled = false;
  document.getElementById("sort").disabled = false;

}

async function selectedBar(index){
  // change the color of the current bar
  let bars = document.getElementsByClassName("array-bar");
  bars[index].style.backgroundColor = "#ff0000";
  // console.log(index);
  await sleep(speed.value);
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

async function merge(left, right, array, fullarray, current_index){
  // helper function for merge sort to merge two arrays
  // console.log("MERGE: merging arrays " + left + " and " + right);
  let left_index = 0;
  let right_index = 0;
  let merged_index = 0;
  while(left_index < left.length && right_index < right.length){ // while there are still elements in both arrays
    playNote(fullarray[merged_index+current_index] * 10, 100);
    await selectedBar(merged_index+current_index);
    if(left[left_index] <= right[right_index]){ // if the left element is less than or equal to the right element, add the left element to the array
      fullarray[merged_index+current_index] = left[left_index];
      array[merged_index] = left[left_index];
      left_index++;
    }
    else{ // otherwise add the right element to the array
      fullarray[merged_index+current_index] = right[right_index];
      array[merged_index] = right[right_index];
      right_index++;
    }
    merged_index++;
  }
  while(left_index < left.length){ // add any remaining elements in the left array to the array
    playNote(fullarray[merged_index+current_index] * 10, 100);
    await selectedBar(merged_index+current_index);
    fullarray[merged_index+current_index] = left[left_index];
    array[merged_index] = left[left_index];
    left_index++;
    merged_index++;
  }
  while(right_index < right.length){ // add any remaining elements in the right array to the array
    playNote(fullarray[merged_index+current_index] * 10, 100);
    await selectedBar(merged_index+current_index);
    fullarray[merged_index+current_index] = right[right_index];
    array[merged_index] = right[right_index];
    right_index++;
    merged_index++;
  }
  // console.log("MERGE: merged array: " + array);
  return array;
}

async function mergeSort(array, fullarray = array, current_index = 0){
  // Merge sort is a sorting algorithm which divides the input array into two halves, calls itself for the two halves, and then merges the two sorted halves. O(nlogn)
    // console.log("merge sort on array: " + array + "\n");
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
    let merged = [];
    merged = await mergeSort(left, fullarray, current_index);
    merged = await mergeSort(right, fullarray, current_index + mid);
    array = await merge(left, right, array, fullarray, current_index);
    return merged; // return the sorted array
}

async function quickSort(array, fullarray = array, current_index = 0){
  // Quick sort is a sorting algorithm which picks an element as pivot and partitions the given array around the picked pivot, elements smaller than pivot go to the left and larger to the right. O(nlogn)
  // console.log("quick sort on array: " + array + "\n");
  let size = array.length;
  if(size < 2){
    return array;
  }
  let pivot = array[size - 1];
  let left = [];
  let right = [];
  for(let i = 0; i < size - 1; i++){ // partition the array
    if(array[i] < pivot){ // if the element is less than the pivot, add it to the left array
      left.push(array[i]);
    }
    else{ // otherwise add it to the right array
      right.push(array[i]);
    }
  }
  // concatenate the left array, the pivot, and the right array for visualization
  let concat_array = left.concat(pivot);
  concat_array = concat_array.concat(right);
  for(let i = 0; i < concat_array.length; i++){
    playNote(fullarray[i+current_index] * 10, 100);
    await selectedBar(i+current_index);
    fullarray[i+current_index] = concat_array[i];
  }
  left = await quickSort(left, fullarray, current_index);
  right = await quickSort(right, fullarray, current_index + left.length + 1);
  for(let i = 0; i < left.length; i++){
    array[i] = left[i];
    fullarray[i+current_index] = left[i];
  }
  array[left.length] = pivot;
  for(let i = 0; i < right.length; i++){
    array[left.length + 1 + i] = right[i];
    fullarray[left.length + 1 + i + current_index] = right[i];
  }
  // console.log("quick sort on array: " + array + "\n");
  return array;
}

async function heapify(array, size, i){
  playNote(array[i] * 10, 100);
  await selectedBar(i);
  // Heapify is a helper function for heap sort which builds a max heap from the input data. O(nlogn)
  let largest = i;
  let left = 2 * i + 1;
  let right = 2 * i + 2;
  if(left < size && array[left] > array[largest]){ // if the left child is larger than the root
    largest = left;
  }
  if(right < size && array[right] > array[largest]){ // if the right child is larger than the root
    largest = right;
  }
  if(largest != i){ // if the largest is not the root
    let temp = array[i];
    array[i] = array[largest];
    array[largest] = temp;
    await heapify(array, size, largest);
  }
}

async function heapSort(array){
  // Heap sort is a sorting algorithm which first builds a max heap from the input data. The largest item is stored at the root followed by a swap with the last item in the array. The heap is then reduced in size by one and the process is repeated until the array is sorted. O(nlogn)
  // console.log("heap sort on array: " + array + "\n");
  let size = array.length;
  for(let i = Math.floor(size / 2) - 1; i >= 0; i--){ // build the max heap
    // playNote(array[i] * 10, 100);
    // await selectedBar(i);
    await heapify(array, size, i);
  }
  for(let i = size - 1; i > 0; i--){ // swap the root with the last item in the array and reduce the heap size by one
    playNote(array[i] * 10, 100);
    await selectedBar(i);
    let temp = array[0];
    array[0] = array[i];
    array[i] = temp;
    await heapify(array, i, 0);
  }
  // console.log("heap sort on array: " + array + "\n");
  return array;

}


</script>

<template>


  <!-- Forms to edit size of array and width of bar -->
  <div class="edit-box">
    <div class="options-wrapper">
      <form>
        <label class="sound-label" for="sound">Mute Sound:
          <div class="switch">
            <input type="checkbox" id="sound" name="sound" v-model="playSounds">
            <span class="slider-toggle round"></span>
          </div>
        </label>
        <label for="size">Size of Array:
          <span id="slider-value">{{array.length}}</span>
        </label>
        <input type="range" class="slider" id="size" name="size" min="1" max="250" v-model="array.length" @input="array = generateRandomArray(array.length)">
      
        <label for="width">Speed of Sorting:
          <span id="slider-value">{{speed}}ms</span>
        </label>
        <input type="range" class="slider" id="speed" name="speed" min="1" max="100" v-model="speed">
        <!-- input for sorting algo -->
        <label for="algo">Sorting Algorithm:</label>
        <select name="algo" id="algo" @input="changeDesctiption()">
          <option value="bubble">Bubble Sort</option>
          <option value="insertion">Insertion Sort</option>
          <option value="selection">Selection Sort</option>
          <option value="merge">Merge Sort</option>
          <option value="quick">Quick Sort</option>
          <option value="heap">Heap Sort</option>
        </select>
      
      </form>
      
      <div class="button-wrapper">
        <button id="sort" @click="sort(array)">Sort!</button>
        <button id="shuffle" @click="array = generateRandomArray(array.length)">Shuffle!</button>
      </div>
    </div>
    
    <!-- Description of sorting algorithm (updates when algo is changed)-->
    <div id="description">
      <div class="description-container">
        <div class="description">
            <h1 id="description-header">{{ algo }}</h1>
            <p id="description-message">{{ msg }}</p>
            <div class="bigo-wrapper">
              <h2>Big-O: </h2>
              <p id="description-bigo">{{ bigO }}</p>
            </div>
        </div>
    </div>
    </div>
    
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
  flex-direction: column;
  justify-content: center;
  align-content: center;
  min-width: 25%;
  max-width: 25%;
  overflow: auto;
}
.options-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.options-wrapper > * {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.button-wrapper {
  flex-direction: row;
  margin: 1rem;
}
.button-wrapper > * {
  margin: 0 0.5rem;
}

label {
  margin: 0.5rem 0;
}
.sound-label {
  margin: 0 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.switch {
  margin-top: 0.5rem;
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider-toggle {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider-toggle:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider-toggle {
  background-color: #2196F3;
}

input:focus + .slider-toggle {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider-toggle:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider-toggle.round {
  border-radius: 34px;
}

.slider-toggle.round:before {
  border-radius: 50%;
}

#slider-value {
  margin-left: 0.5rem;
}

select {
  margin: 0.5rem 0;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.5rem;
  background-color: #ffffff;
  color: #a10606;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
}



button {
  margin: 0.5rem 0;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 0.5rem;
  background-color: #ffffff;
  color: #a10606;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: 0.2s;
}
button:hover {
  background-color: #c5c5c5;
}
button:disabled {
  opacity: 0.75;
}


.description-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    background-color: #ffffff;
    border-radius: 10px;
    margin-top: 10px;
}

.description {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: #ffffff;
    padding: 10px;
    border-radius: 10px;
    width: 75%;
}

.description h1 {
    color: #a10606;
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 20px;
}

.bigo-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
}

.description h2 {
    color: #a10606;
    font-size: 1.5rem;
    font-weight: 700;
    margin: 0;
    padding-right: 10px;
}
.description #description-bigo {
    color: #a10606;
    font-size: 1.2rem;
    font-weight: 500;
}

.description p {
    color: #a10606;
    font-size: 1.2rem;
    font-weight: 400;
    margin-bottom: 15px;
}


@media only screen and (max-width: 600px) {
  .edit-box {
    max-width: 100%;
    width: 100%;
    height: 30%;
    min-height: 50%;
    padding: 0;
    align-items: center;
  }
  .edit-box form {
    width: 50%;
    height: 70%;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-evenly;
    align-content: center;
  }
  .description-container {
    display: none;
  }
  
}

</style>

