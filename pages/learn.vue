<script setup>
const localePath = useLocalePath()

</script>
<template>
  <div class="max-w-4xl mx-auto p-6 text-white mt-6 sm:mt-12">
    <h1 class="text-3xl font-bold text-center">Nsite Basics</h1>
    
    <h2 class="text-2xl font-semibold mt-6">What is an Nsite?</h2>
    <p class="mt-2">
      The concept of <strong>Nsite</strong> was born during the second cohort of 
      <a href="https://sovereignengineering.io/" class="text-purple-400 underline hover:underline">Sovereign Engineering</a>. 
      It utilizes <strong>Nostr</strong> and <strong>Blossom servers</strong> to deploy static websites and clients.
    </p>
    <p class="mt-2">
      Nsite makes it easy to deploy websites to multiple hosts in a <strong>non-KYC</strong> way, 
      allowing payments via Bitcoin while ensuring redundancy is built-in.
    </p>
    <p class="mt-2">
      Another key feature is <strong>Nsite Gateways</strong>: 
      Even if your website is blocked on one domain, it remains accessible through different gateways 
      because these sites resolve via <strong>Nostr</strong>.
    </p>
    
    <h3 class="text-xl font-semibold mt-4">Summary:</h3>
    <ul class="list-disc list-inside mt-2 space-y-2">
      <li><strong>Censorship Resistance</strong> through redundancy</li>
      <li><strong>Non-KYC Hosting Solutions</strong></li>
      <li><strong>Multi-Gateway Resolution</strong></li>
      <li><strong>No Need for GitHub</strong> – Store, edit, and deploy your website directly to <strong>Blossom servers</strong></li>
    </ul>
    
    <hr class="my-6 border-gray-600" />
    
    <h2 class="text-2xl font-semibold mt-6">Where Do I Get Started?</h2>
    <p class="mt-2">
      We have compiled a list of tools to help you deploy your website. You can find them here:
    </p>
    <p class="mt-2"><NuxtLink :to="localePath('/tools')" class="flex underline text-purple-500">🔗 Nsite Tools</NuxtLink></p>
    <p class="mt-2">If you're experiencing deployment issues, visit:</p>
    <p class="mt-2"><NuxtLink :to="localePath('/debug')" class="flex underline text-purple-500">🔗 Nsite Debug</NuxtLink></p>
    
    <hr class="my-6 border-gray-600" />
    
    <h2 class="text-2xl font-semibold mt-6">Technical Implementation</h2>
    <ol class="list-decimal list-inside mt-2 space-y-2">
      <li>
        <strong>Generate a Nostr Key Pair</strong> <br> The <strong>private key</strong> is used to upload your website’s public folder 
        to one or multiple <strong>Blossom servers</strong> (e.g., <code>BUD-03</code>, <code>10063</code>).
      </li>
      <li>
        <strong>Upload & Broadcast</strong> <br> Once uploaded, <strong>notes</strong> (<code>Kind: 34128</code>) containing the 
        <strong>filename</strong> and <strong>file hash</strong> are broadcasted to your <strong>Nostr Outbox Relays</strong> 
        (<code>NIP 65</code>, <code>Kind: 10002</code>).
      </li>
      <li>
        <strong>No More GitHub Needed</strong> <br> Since the website’s code is stored on <strong>Blossom servers</strong>, 
        there’s no need for GitHub. You can <strong>edit, deploy, and store</strong> your website directly on Blossom servers, 
        eliminating dependency on centralized version control platforms.
      </li>
    </ol>
    
    <hr class="my-6 border-gray-600" />
    
    <h2 class="text-2xl font-semibold mt-6">Main Critique</h2>
    <p class="mt-2">
      Currently, most sites still rely on the <strong>DNS backbone</strong>. However, it is possible to resolve them via 
      <strong>gateways hosted over TOR</strong>.
    </p>
    <p class="mt-2">
      The primary goal is to create websites that can resolve <strong>natively over the clearnet</strong>.
    </p>

    <Footer />


  </div>
</template>
