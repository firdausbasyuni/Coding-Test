<template>
    <div class="flex flex-col items-center p-4 min-h-screen bg-gray-100">
      <h1 class="text-xl font-bold mb-4">Text Converter</h1>
  
      <input
        v-model="inputText"
        type="text"
        placeholder="Enter text"
        class="p-2 border rounded w-80 mb-3"
      />
  
      <button
        @click="processConversion"
        class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
      >
        Convert
      </button>
  
      <div v-if="convertedNumbers.length" class="mt-4 text-sm">
        <p><strong>1. Converted Numbers:</strong> {{ convertedNumbers.join(" ") }}</p>
        <p><strong>2. Final Sum:</strong> {{ finalSum }}</p>
        <p><strong>3. Converted Letters:</strong> {{ finalLetters.join(" ") }}</p>
      </div>
    </div>
</template>
  
<script setup>
import { ref } from "vue";

const inputText = ref("");
const convertedNumbers = ref([]);
const finalSum = ref(0);
const finalLetters = ref([]);

const charToNumMap = {
  A: 0, B: 1, C: 1, D: 1, E: 2, F: 3, G: 3, H: 3, I: 4, 
  J: 5, K: 5, L: 5, M: 5, N: 5, O: 6, P: 7, Q: 7, R: 7, S: 7, T: 7, 
  U: 8, V: 9, W: 9, X: 9, Y: 9, Z: 9,

  a: 9, b: 8, c: 8, d: 8, e: 7, f: 6, g: 6, h: 6, i: 5, 
  j: 4, k: 4, l: 4, m: 4, n: 4, o: 3, p: 2, q: 2, r: 2, s: 2, t: 2, 
  u: 1, v: 0, w: 0, x: 0, y: 0, z: 0, " ": 0
};

const convertTextToNumbers = (text) => {
  return text.split("").map(char => charToNumMap[char] ?? 0);
};

const alternateSum = (numbers) => {
  if (numbers.length === 0) return 0;

  let sum = numbers[0];
  for (let i = 1; i < numbers.length; i++) {
    sum = i % 2 === 1 ? sum + numbers[i] : sum - numbers[i];
  }
  return sum;
};

const generateSumSequence = (num) => {
  let sequence = [];
  let sum = 0;
  let nextValue = 0;

  while (sum < num) {
    if (sum + nextValue > num) {
      nextValue = 0;
    }
    sequence.push(nextValue);
    sum += nextValue;
    nextValue++;
  }

  return sequence;
};

const convertNumberToLetters = (num) => {
  let sequence = generateSumSequence(Math.abs(num));
  
  return sequence.map(d => {
    let matchingLetters = Object.keys(charToNumMap)
      .filter(char => charToNumMap[char] === d && char === char.toUpperCase());
    return matchingLetters.length > 0 ? matchingLetters[0] : "";
  });
};

const processConversion = () => {
  if (!inputText.value) return;

  convertedNumbers.value = convertTextToNumbers(inputText.value);
  finalSum.value = alternateSum(convertedNumbers.value);
  finalLetters.value = convertNumberToLetters(finalSum.value);
};
</script>