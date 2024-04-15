<template>
  <div class="w-screen h-full bg-neutral-900 text-white">
    <div class="m-4">
      <header class="w-full h-20 border-4 border-neutral-700 rounded-xl flex items-center justify-center gap-16 p-4 font-semibold text-2xl">
        <h1 class="absolute left-8">SP3POW</h1>
        <button @click="loadMission('Instruktarzowa', 'https://www.youtube.com/embed/4n8DRxDchJg?autoplay=1&mute=0&color=white&fs=1&version=3&loop=1&playlist=4n8DRxDchJg', '../flights/First/RTTY/SP3POW.json', 3, 2)" class="shadow-4 drop-shadow-lg hover:text-shadow-[5px_5px_rgba(255,_255,_255,_0.1),-5px_5px_rgba(255,_255,_255,_0.1),_10px_10px_rgba(255,_255,_255,_0.075),_-10px_10px_rgba(255,_255,_255,_0.075),_15px_15px_rgba(255,_255,_255,_0.05),_-15px_15px_rgba(255,_255,_255,_0.05),_20px_20px_rgba(255,_255,_255,_0.025),_-20px_20px_rgba(255,_255,_255,_0.025),_25px_25px_rgba(255,_255,_255,_0.0125),_-25px_25px_rgba(255,_255,_255,_0.0125)]">MISJA INSTRUKTARZOWA</button>
        <button @click="loadMission('Larwa 1', 'https://www.youtube.com/embed/grEeLXQ1o_8?autoplay=1&mute=0&color=white&fs=1&version=3&loop=1&playlist=grEeLXQ1o_8', '../flights/Larwa1/RTTY/SP3POW.json', 6, 5)" class="shadow-4 drop-shadow-lg hover:text-shadow-[5px_5px_rgba(255,_255,_255,_0.1),-5px_5px_rgba(255,_255,_255,_0.1),_10px_10px_rgba(255,_255,_255,_0.075),_-10px_10px_rgba(255,_255,_255,_0.075),_15px_15px_rgba(255,_255,_255,_0.05),_-15px_15px_rgba(255,_255,_255,_0.05),_20px_20px_rgba(255,_255,_255,_0.025),_-20px_20px_rgba(255,_255,_255,_0.025),_25px_25px_rgba(255,_255,_255,_0.0125),_-25px_25px_rgba(255,_255,_255,_0.0125)]">MISJA LARWA 1</button>
        <button @click="loadMission('Larwa 2', 'https://www.youtube.com/embed/cEvkfN12RwU?autoplay=1&mute=0&color=white&fs=1&version=3&loop=1&playlist=cEvkfN12RwU', '../flights/Larwa2/RTTY/SP3POW.json', 6, 5)" class="shadow-4 drop-shadow-lg hover:text-shadow-[5px_5px_rgba(255,_255,_255,_0.1),-5px_5px_rgba(255,_255,_255,_0.1),_10px_10px_rgba(255,_255,_255,_0.075),_-10px_10px_rgba(255,_255,_255,_0.075),_15px_15px_rgba(255,_255,_255,_0.05),_-15px_15px_rgba(255,_255,_255,_0.05),_20px_20px_rgba(255,_255,_255,_0.025),_-20px_20px_rgba(255,_255,_255,_0.025),_25px_25px_rgba(255,_255,_255,_0.0125),_-25px_25px_rgba(255,_255,_255,_0.0125)]">MISJA LARWA 2</button>
      </header>
    </div>
    <main class="flex flex-wrap">
      <div class="flex flex-wrap w-1/2">
        <div class="w-full p-4 pr-2 pt-0">
          <div class="bg-neutral-700 rounded-xl flex flex-col p-2">
            <iframe :src="currentMission.videoURL" title="Misja Video" frameborder="1" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen class="w-full h-auto aspect-video rounded-xl"></iframe>
            <h2 class="text-2xl mt-2 font-medium">Filmik z wypuszczenia balonu stratosferycznego</h2>
          </div>
        </div>
        <div class="w-1/2 p-4 pr-2 pt-0">
          <div class="p-2 bg-neutral-700 rounded-lg">
            <div id="chart4" class="bg-neutral-700 rounded-lg"></div>
            <h2 class="text-2xl mt-2  font-semibold">Wykres napięcia [V] do czasu</h2>
          </div>
        </div>
        <div class="w-1/2 p-4 pl-2 pr-2 pt-0">
          <div class="p-2 bg-neutral-700 rounded-lg">
            <div id="chart3" class="bg-neutral-700 w-full rounded-lg"></div>
            <h2 class="text-2xl mt-2  font-semibold">Kto odebrał ile ramek</h2>
          </div>
        </div>
      </div>
      <div class="w-1/2 h-auto p-4 pl-2 pt-0 rounded-lg flex flex-col">
        <div class="w-auto h-full p-2 bg-neutral-700 rounded-lg" style="margin-bottom: 2px;">
          <div class="w-full rounded-xl" style="height: 96.5%;">
            <l-map :zoom="7" :center="[52, 21]" :use-global-leaflet="false" class="rounded-xl">
              <l-tile-layer class="rounded-xl" url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" layer-type="base" name="OpenStreetMap"></l-tile-layer>
              <l-polyline :lat-lngs="routePoints" color="red" :weight="3"></l-polyline>
            </l-map>
            <h2 class="text-2xl mt-2 mb-2 font-semibold">Mapa pokazująca trasę lotu balonu</h2>
          </div>
        </div>
      </div>
      <div class="w-1/2 h-96 p-4 pr-2 pt-0">
        <div class="w-full h-full bg-neutral-700 rounded-lg">
          <div class="p-2 bg-neutral-700 rounded-lg">
            <div id="chart" class="bg-neutral-700 w-full rounded-lg"></div>
            <h2 class="text-2xl mt-2  font-semibold">Wykres temperatury [°C] do czasu</h2>
          </div>
        </div>
      </div>
      <div class="w-1/2 h-96 p-4 pl-2 pt-0">
        <div class="w-full h-full bg-neutral-700 rounded-lg">
          <div class="p-2 bg-neutral-700 rounded-lg">
            <div id="chart2" class="bg-neutral-700 w-full rounded-lg"></div>
            <h2 class="text-2xl mt-2  font-semibold">Wykres wysokości [m] do czasu</h2>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import Plotly from 'plotly.js-dist';
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LPolyline } from "@vue-leaflet/vue-leaflet";

const data = ref(null);
const data2 = ref(null);
const routePoints = ref([]);
const data3 = ref(null);
const data4 = ref(null);

const currentMission = ref({
  videoURL: 'https://www.youtube.com/embed/4n8DRxDchJg?autoplay=1&mute=0&color=white&fs=1&version=3&loop=1&playlist=4n8DRxDchJg',
  jsonURL: '../flights/First/RTTY/SP3POW.json',
  raw: '3',
  voltage: '1'
});

const loadMission = (missionName, videoURL, jsonURL, raw, voltage) => {
  currentMission.value.videoURL = videoURL;
  currentMission.value.jsonURL = jsonURL;
  currentMission.value.raw = raw;
  currentMission.value.voltage = voltage;
  loadData();
};

const loadData = async () => {
  const response = await fetch(currentMission.value.jsonURL);
  let json = await response.json();

  const uploaderData = {};
  json.forEach(item => {
    const uploader = item.uploader_callsign;
    if (!uploaderData[uploader]) {
      uploaderData[uploader] = 0;
    }
    uploaderData[uploader]++;
  });

  const uploaderNames = Object.keys(uploaderData);
  const uploaderCounts = Object.values(uploaderData);

  routePoints.value = json.map(item => [item.lat, item.lon]);

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
    return parseFloat(raw[raw.length - currentMission.value.raw]);
  });

  const speed = json.map(item => {
    const raw = item.raw.split(',');
    return parseFloat(raw[raw.length - currentMission.value.raw]);
  });

  const voltage = json.map(item => {
    const raw = item.raw.split(',');
    return parseFloat(raw[raw.length - currentMission.value.voltage]);
  });

  const time = json.map(item => new Date(item.datetime));
  const altitude = json.map(item => item.alt);

  data.value = [
    {
      x: time,
      y: temperature,
      mode: 'markers',
      line: { shape: 'spline'},
      type: 'scatter',
      marker: { color: '#10b981' }
    }
  ];

  data2.value = [
    {
      x: time,
      y: altitude,
      mode: 'lines',
      line: { shape: 'spline', color: '#10b981' }, // zmiana koloru linii
      type: 'scatter',
      marker: { color: '#404040' } // zmiana koloru tła wykresu
    }
  ];

  
  data3.value = [
  {
      labels: uploaderNames,
      values: uploaderCounts,
      type: 'pie',
      marker: { colors: ['#ff4500', '#ffa500', '#ffd700', '#32cd32', '#4682b4', '#9400d3'] }, // Kolory dla poszczególnych kategorii
      textinfo: 'label+value', // Wyświetlanie nazw i procentów na kawałkach ciasta
      textfont: { weight: 'black' } // Pogrubiony tekst
    }
  ];

  
  data4.value = [
    {
      x: time,
      y: voltage,
      mode: 'lines',
      line: { shape: 'spline', color: '#10b981' }, // zmiana koloru linii
      type: 'scatter',
      marker: { color: '#404040' } // zmiana koloru tła wykresu
    }
  ];

  Plotly.react('chart', data.value, {
    margin: { t: 0, b: 30, l: 38, r: 30 }, // Usuwamy margines górny, ale zostawiamy miejsce na opis osi y
    dragmode: false, // Wyłączamy możliwość przesuwania wykresu
    displayModeBar: false, // Ukrywamy pasek narzędziowy
    plot_bgcolor: '#404040', // zmiana koloru tła wykresu
    paper_bgcolor: '#404040', // zmiana koloru tła obszaru wykresu
    font: { color: 'white' }, // zmiana koloru tekstu
    xaxis: { gridcolor: '#555' }, // Zmiana koloru linii pomocniczych dla osi x
    yaxis: { gridcolor: '#555', title: { text: 'Temperatura [°C]', font: { color: 'white' } }, automargin: true  } // Zmiana koloru linii pomocniczych dla osi y
  });
  Plotly.react('chart2', data2.value, {
    margin: { t: 0, b: 30, l: 50, r: 30 }, // Usuwamy margines górny, ale zostawiamy miejsce na opis osi y
    dragmode: false, // Wyłączamy możliwość przesuwania wykresu
    displayModeBar: false, // Ukrywamy pasek narzędziowy
    plot_bgcolor: '#404040', // zmiana koloru tła wykresu
    paper_bgcolor: '#404040', // zmiana koloru tła obszaru wykresu
    font: { color: 'white' }, // zmiana koloru tekstu
    xaxis: { gridcolor: '#555' }, // Zmiana koloru linii pomocniczych dla osi x
    yaxis: { gridcolor: '#555', title: { text: 'Wysokość [m]', font: { color: 'white' } }, automargin: true } // Zmiana koloru linii pomocniczych dla osi y
  });
  Plotly.react('chart3', data3.value, {
    margin: { t: 0, b: 30, l: 50, r: 30 }, // Usuwamy margines górny, ale zostawiamy miejsce na opis osi y
    dragmode: false, // Wyłączamy możliwość przesuwania wykresu
    displayModeBar: false, // Ukrywamy pasek narzędziowy
    plot_bgcolor: '#404040', // zmiana koloru tła wykresu
    paper_bgcolor: '#404040', // zmiana koloru tła obszaru wykresu
    font: { color: 'white' }, // zmiana koloru tekstu
  });
  Plotly.react('chart4', data4.value, {
    margin: { t: 0, b: 30, l: 50, r: 30 }, // Usuwamy margines górny, ale zostawiamy miejsce na opis osi y
    dragmode: false, // Wyłączamy możliwość przesuwania wykresu
    displayModeBar: false, // Ukrywamy pasek narzędziowy
    plot_bgcolor: '#404040', // zmiana koloru tła wykresu
    paper_bgcolor: '#404040', // zmiana koloru tła obszaru wykresu
    font: { color: 'white' }, // zmiana koloru tekstu
    xaxis: { gridcolor: '#555' }, // Zmiana koloru linii pomocniczych dla osi x
    yaxis: { gridcolor: '#555', title: { text: 'Napiecie [V]', font: { color: 'white' } }, automargin: true } // Zmiana koloru linii pomocniczych dla osi y
  });
};

onMounted(loadData);
</script>