<template>
  <div class="max-w-4xl mx-auto p-6 text-white mt-6 sm:mt-12">
    <h1 class="text-3xl font-bold text-center">Nsite-Tools</h1>
    <p class="text-center mt-2">
      A list of useful tools and repositories to deploy and debug Nsite's.
    </p>
    <p class="text-center mt-2">
      These are built by some great folks: 
      <a href="https://github.com/lez" class="text-purple-400 hover:underline">Lez</a>,
      <a href="https://github.com/flox1an" class="text-purple-400 hover:underline">flox1an</a>, 
      <a href="https://github.com/hzrd149" class="text-purple-400 hover:underline">hzrd149</a>
    </p>
    
    <h2 class="text-2xl font-semibold mt-6">Nsite Uploaders</h2>
    <ul class="mt-2 space-y-2">
      <li><a href="https://www.npmjs.com/package/nsite-cli" class="text-purple-400 hover:underline">nsite-cli</a> - A NodeJS CLI tool to upload Nsite's.</li>
      <li><a href="https://github.com/lez/nsite" class="text-purple-400 hover:underline">Nsite Python Tool</a> - A Python tool to upload Nsite's.</li>
      <li><a href="https://bouquet.slidestr.net" class="text-purple-400 hover:underline">Bouquet</a> - A blossom server file upload manager.</li>
    </ul>

    <h2 class="text-2xl font-semibold mt-6">Nsite Groupchat</h2>
    <p class="mt-2"><a href="https://chachi.chat/groups.hzrd149.com/e23891" class="text-purple-400 hover:underline">Join the Nostr Nsite Groupchat</a> to discuss all Nsite related things!</p>
    
    <h2 class="text-2xl font-semibold mt-6">Nsite Gateways</h2>
    <ul class="mt-2 space-y-2">
      <li><a href="https://nsite.lol" class="text-purple-400 hover:underline">Nsite.lol</a> - A Nsite explorer and gateway to resolve deployed Nsites.</li>
      <li><a href="https://nsite.cloud" class="text-purple-400 hover:underline">Nsite.cloud</a> - A Nsite resolver and editor (WIP) and Nsite template loader (WIP).</li>
    </ul>
    
    <h2 class="text-2xl font-semibold mt-6">Nsite Debug</h2>
    <p class="mt-2"><a href="https://nsite.info" class="text-purple-400 hover:underline">Nsite.info</a> - A collection of tools and information regarding the Nsite concept, also hosts a simple Nsite Debug overview.</p>
    
    <h2 class="text-2xl font-semibold mt-6">Nsite Info Repository List</h2>
    <ul class="mt-2 space-y-2">
      <li><strong>Nsite-Basics</strong> <br> Simple Markdown explanation of the Nsite concept.</li>
      <li><strong>Nsite-Tools</strong> <br> Simple Markdown list of tools and repositories to help explore Nsite's.</li>
      <li><strong>Nsite-Debug</strong> <br> A NuxtJS application and site to help debug current settings for an Npub, hosted on Nsite.info.</li>
      <li><strong>Nsite-Upload</strong> <br> A simple example repo of a single HTML page you can upload using your Nsec, using the Nsite NodeJS CLI.</li>
      <li><strong>Nsite-Gateway (WIP)</strong> <br> A Nsite gateway that can be configured as a full resolver for Npub subdomains or as a personal Npub/Nsite resolver.</li>
      <li><strong>Nsite-Info-Website</strong> <br> A website hosting the Nsite Basics, Tools & Debugger repositories.</li>
      <li><strong>Nsite-Cloud-Website (WIP)</strong> <br> A website that hosts an Nsite Editor, Template deployment, and curated list of Nsite Examples; also hosts the Nsite Gateway repo.</li>
    </ul>

    <Footer />


  </div>
</template>