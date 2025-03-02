<template>
    <div class="p-6 ">
      <div v-if="nsites.length" class="grid grid-cols-1 md:grid-cols-3 gap-6">
  <a 
    v-for="site in nsites" 
    :key="site.npub" 
    :href="'https://' + site.npub + '.nsite.lol'" 
    target="_blank" 
    class="no-underline bg-purple-800 p-6 rounded-lg shadow-lg border-4 border-gray-200 hover:bg-purple-700 transition"
  >
    <div class="flex items-center gap-4 mb-3">
      <h3 class="font-semibold text-lg text-white">{{ site.name }}</h3>
      <img 
        v-if="site.logo" 
        :src="site.logo" 
        alt="Website Logo" 
        class="h-16 w-16 object-cover rounded-full border border-gray-300"
      />
    </div>
    <p class="text-sm text-white truncate">Npub: <span class="font-mono">{{ site.npub }}</span></p>
  </a>
</div>

      <p v-else class="text-white">No Nsites available. Click the button to fetch them.</p>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const nsites = ref([
  { name: 'SovEng', npub: 'npub1sxxtj6cm9a4jjnf2wfjzzjmmdqcuz06xs2hh067n3k598dpq0dmsq5725g', lastUpdate: '11-2-2025, 20:16:39', logo: 'https://image.nostr.build/02e4cc16c94ea7f7ea6c89569cef8decc630d3ef8899aa58a5dedb7384f4dea5.jpg' },

    { name: 'Slidestr', npub: 'npub1m2vnh8fze2vqka4y54e7dl89l6pfhfg2zh2aw822etplyrtvaqjqtppm4a', logo: 'https://cdn.satellite.earth/f6f748ac94361bc7454464d651c4caae28a540642f103712ade281da07c4ae7d.svg' },
    { name: 'NoStrudel', npub: 'npub1wyuh3scfgzqmxn709a2fzuemps389rxnk7nfgege6s847zze3tuqfl87ez', logo: 'https://cdn.hzrd149.com/303f018e613f29e3e43264529903b7c8c84debbd475f89368cb293ec23938981.png' },
    { name: 'Novia', npub: 'npub126uz2g6ft45qs0m0rnvtvtp7glcfd23pemrzz0wnt8r5vlhr9ufqnsmvg8', logo: 'https://pfp.nostr.build/2811e73c424ddffada90fbe3b5ec51235d5b326ab11f7b5365c223c771209a80.png' },
    { name: 'Bouquet', npub: 'npub1uch500jz4kknlzg4h9y3yv3d42us792ru52actv7kt8rhxg0xnlscctm5r', logo: 'https://cdn.nostrcheck.me/fb347648ff902486501f5d6d12c3f47e0350fcfc8c9510c48b341daa7bb25063.png' },
    { name: 'SovBiz', npub: 'npub1s0v8lzxhgzrcp039pqw5fwt99grhnh0hdl7atcyxl4lkpkxy076smu0xev', logo: 'https://image.nostr.build/d252e3afba4e6ee5525a200cdcf45c147af7856a1a49b3ab39f3bf64a6425328.png' }
  ]);
  </script>
  