<script setup>

import { ExclamationTriangleIcon } from '@heroicons/vue/20/solid'

import data from "~/config/setup";

const localePath = useLocalePath()

const searchTerm = ref('');

const hex = ref('');


import { bech32 } from 'bech32';

import debounce from 'debounce';
import NDK from '@nostr-dev-kit/ndk'; // Corrected import statement

// Utility functions
const transparentPixel = 'data:image/gif;base64,R0lGODlhAQABAAD/ACwAAAAAAQABAAACADs=';  // 1x1 transparent pixel
const handleImageError = (event) => {
  event.target.src = transparentPixel;
};

// Nostr relays
const relays = [
  'wss://nostr.wine',
  'wss://search.nos.today',
  'wss://relay.nostr.band',
  'wss://relay.noswhere.com',
];

// Initialize NDK with relays
const ndk = new NDK({ explicitRelayUrls: relays });

// State variables
// const searchTerm = ref('');
const users = ref([]);
let currentSubscription = null;  // Track the current subscription

// Function to clear the users array
const clearUsers = () => {
  users.value = [];
};

// Search function using NDK
const searchUsers = async (query) => {
  if (query.length < 2) {
    clearUsers();
    return;
  }

  const newUsers = new Set();  // Use a Set to handle duplicates
  const filter = { kinds: [0], search: query }; // Kinds [0] for metadata

  // Unsubscribe from previous search if it exists
  if (currentSubscription) {
    currentSubscription.stop();  // Stop the previous subscription
  }

  // Create a new subscription
  currentSubscription = ndk.subscribe(filter, { relays });

  currentSubscription.on('event', (event) => {
    try {
      const metadata = JSON.parse(event.content);
      const user = {
        pubkey: event.pubkey,
        npub: event.pubkey,  // You can encode this if needed
        name: metadata.name || 'Unknown',
        picture: metadata.picture,
      };

      // Add to Set to avoid duplicates
      newUsers.add(JSON.stringify(user));  // Serialize to use Set

      // Limit to 10 results
      if (newUsers.size > 10) {
        const entries = Array.from(newUsers).slice(0, 10);
        users.value = entries.map(JSON.parse);  // Deserialize back to objects
      } else {
        users.value = Array.from(newUsers).map(JSON.parse);  // Update users
      }
    } catch (error) {
      console.error('Error parsing event:', error);
    }
  });

  // Close the connection after 10 seconds
  setTimeout(() => {
    if (currentSubscription) {
      currentSubscription.stop();  // Stop the subscription
      currentSubscription = null;
    }
  }, 10000); // 10 seconds
};

// Debounced search function
const debouncedSearch = debounce(() => searchUsers(searchTerm.value), 450);

// Handle user selection
const selectUser = async (user) => {
  try {
    console.log(user.name)
    const response = await fetch(`https://register.sov.biz/nostrnip5/api/v1/domain/eaz5cgZEbGd8xGv9ZzymtN/nostr.json?name=${user.name}`);

    const data = await response.json();
    const hex = data.names?.[user];

    if (hex && hex !== "Not found") {
      console.log("we did find a username routing to the username " + hex.value)
      window.location.href = "https://"+ convertHexToNpub(user.npub) + ".nsite.info";

      // Redirect to npub subdomain
      // window.location.href = "https://"+ convertHexToNpub(user.npub) + ".cypher.space";
    } else {
      // Redirect to user subdomain if no match
      console.log("we didn't find a username falling back to npub route ")
      window.location.href = "https://"+ convertHexToNpub(user.npub) + ".nsite.info";

      // window.location.href = "https://"+ user.name + ".cypher.space";
    }
  } catch (error) {
    // Handle fetch or processing errors by redirecting
    console.log("Error Handle Falling back to npub route ")
    window.location.href = "https://"+ convertHexToNpub(user.npub) + ".nsite.info";

    // window.location.href = "https://"+ convertHexToNpub(user.npub) + ".cypher.space";
  }
};


// Watch for changes in search term and unsubscribe if needed
watch(searchTerm, (newVal) => {
  if (newVal.length < 2 && currentSubscription) {
    currentSubscription.stop();
    currentSubscription = null;
    clearUsers();
  }
});

// Connect to NDK relays when the component is created
ndk.connect();


const hexToByteArray = (hex) => {
  const bytes = [];
  for (let i = 0; i < hex.length; i += 2) {
    bytes.push(parseInt(hex.substr(i, 2), 16));
  }
  return bytes;
};

const convertHexToNpub = (hex) => {
  try {
    const byteArray = hexToByteArray(hex);
    const words = bech32.toWords(byteArray); // Convert byte array to Bech32 words
    const npub = bech32.encode('npub', words); // Encode using the 'npub' prefix
    return npub;
  } catch (error) {
    console.error('Error converting hex to npub:', error);
    return null;
  }
};

const lookupNpub = async (user, domain) => {
  try {
    const response = await fetch(
      `https://register.sov.biz/nostrnip5/api/v1/domain/eaz5cgZEbGd8xGv9ZzymtN/nostr.json?name=${user}`
    );
    if (response.ok) {
      const data = await response.json();
      console.log("We Found a username at register" + data.names[user]);

      const hex = data.names[user];

      if (hex !== "Not found") {
        const byteArray = hexToByteArray(hex);
        const words = bech32.toWords(byteArray);
        const npub = bech32.encode('npub', words);
        
        window.location.href = `https://${npub}.nsite.info`;

        console.log(`npub: ${npub}`);
      } else {
        console.log('Hex not found');
      }
    } else {
      console.log('Error fetching data');
    }
  } catch (error) {
    console.log('Error fetching data:', error);
  }
};


const resolve = () => {
  // Trim spaces from the input value
  searchTerm.value = searchTerm.value.trim();

  const inputLength = searchTerm.value.length;
  if (searchTerm.value == '') {
    console.log("It's an empty world");
    alert("Sovereign Space can't be Empty")
  } 
  else if (searchTerm.value.includes('@')) {
    console.log("Slug contains @ symbol.");
    try {
      const [user, domain] = searchTerm.value.split("@");
      lookupNpub(user, domain).then(() => {});
    } catch (error) {
      console.error("Failed to fetch user profile:", error);
    }
  } else if (searchTerm.value.startsWith('npub')) {
    console.log("Slug starts with npub.");
    try {
      window.location.href = `https://${searchTerm.value}.nsite.info`;
    } catch (error) {
      console.error("Failed to fetch user profile:", error);
    }
  } else if (inputLength < 25) {
    console.log("Input is less than 25 characters.");
    try {
      window.location.href = `https://${searchTerm.value}.nsite.info`;
    } catch (error) {
      console.error("Error processing input less than 25 characters:", error);
    }
  } else if (inputLength > 25) {
    console.log("Input is more than 25 characters.");
    try {
      window.location.href = `https://nsite.info/${searchTerm.value}`;
    } catch (error) {
      console.error("Error processing input more than 25 characters:", error);
    }
  } else {
    console.log("Input does not match any conditions.");
  }
};

// Function to handle the Enter key press
const handleKeyPress = (event) => {
  if (event.key === 'Enter') {
    resolve();
  }
};
</script>

<template>
  <div class="relative w-full  flex flex-col">
    
    <div class="relative z-10 flex-grow px-6">
      <div class="mx-auto max-w-4xl py-0 sm:py-12 lg:py-12">
        <div class="text-center">


          <input 
            type="text" 
            v-model="searchTerm"
            placeholder="Search on Npub or Name" 
            @input="debouncedSearch"
            @keydown="handleKeyPress"
            class="dark:text-black w-full h-16 rounded-2xl px-4 shadow-xl shadow-white/10"
          />
          
          <!-- Search Results -->
          <ul v-if="users.length" class="divide-y divide-gray-200 rounded-2xl bg-gray-900 max-h-72 overflow-y-auto mt-6">
            <li
              v-for="user in users"
              :key="user.pubkey"
              @click="selectUser(user)"
              class="flex items-center p-2 cursor-pointer hover:bg-gray-100 transition hover:text-black text-white"
            >
              <img
                :src="user.picture || transparentPixel"
                @error="handleImageError"
                class="w-10 h-10 mr-2 rounded-full"
              />
              <span class="flex-1 truncate">{{ user.name }} {{ convertHexToNpub(user.npub) }}</span>
            </li>
          </ul>

          <button @click="resolve" class="mt-6 h-12 bg-purple-600 hover:bg-purple-700 text-base border-2 text-black text-white font-bold py-2 px-4 rounded shadow-xl shadow-white/20">Search</button>

        </div>
      </div>
    </div>
  </div>
</template>

