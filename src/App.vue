<!-- <template>
  <div class="w-screen p-4 h-full">
    <header class="w-full h-20 border-4 border-gray-400 rounded-xl flex items-center justify-center gap-16 p-4 font-semibold text-2xl">
      <h1 class="absolute left-8">SP3POW</h1>
      <button>MISJA INSTRUKTARZOWA</button>
      <button>MISJA LARWA 1</button>
      <button>MISJA LARWA 2</button>
    </header>
    <div class="bg-gray-400 w-1/2 mt-4 rounded-xl">
      <iframe src="https://www.youtube.com/embed/4n8DRxDchJg?si=aotTa7JIf5ZKNyjL&amp;controls=0" title="Misja Instruktarzowa" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen class="w-full h-auto aspect-video rounded-xl"></iframe>
    </div>
    <div class="h-96 w-1/2">

    </div>
    <div id="chart" class=" bg-slate-800 w-1/2"></div>
    <div id="chart2" class=" bg-slate-800  w-1/2"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Plotly from 'plotly.js-dist';
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";

const zoom = ref(13);
const center = ref([51.505, -0.09]);
const markerPosition = ref([51.505, -0.09]);
const data = ref(null);
const data2 = ref(null);

onMounted(async () => {
  const response = await fetch('../flights/23.03.2024/RTTY/SP3POW.json');
  let json = await response.json();

  const frames = new Set();
  json = json.filter(item => {
    if (frames.has(item.frame)) {
      return false;
    } else {
      frames.add(item.frame);
      return true;
    }
  });

  const temperature = json.map(item => {
    const raw = item.raw.split(',');
    return parseFloat(raw[raw.length - 6]);
  });

  const time = json.map(item => new Date(item.datetime));
  const altitude = json.map(item => item.alt);

  data.value = [
    {
      x: time,
      y: temperature,
      mode: 'markers',
      line: { shape: 'spline' },
      type: 'scatter'
    }
  ];

  data2.value = [
    {
      x: time,
      y: altitude,
      mode: 'lines',
      line: { shape: 'spline' },
      type: 'scatter',
      marker: { color: temperature, colorscale: 'RdBu' }
    }
  ];

  Plotly.newPlot('chart', data.value);
  Plotly.newPlot('chart2', data2.value);
});
</script> -->

<template>
  <div class="w-screen h-screen" id="app">
    <p>123</p>
    <l-map :zoom="14" :center="[51, 17]">
      <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"></l-tile-layer>
    </l-map>
  </div>
</template>

<script setup>
import { LMap, LTileLayer } from '@vue-leaflet/vue-leaflet';
</script>
