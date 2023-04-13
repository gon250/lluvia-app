<script>
    import { onMount, tick } from 'svelte';


    import heavyRain from './sounds/heavy-rain.wav';
    import rainFromACar from './sounds/rain-from-a-car.wav';
    import rainBeach from './sounds/rain-beach.wav';
    import rainForest from './sounds/rain-forest.wav';

    let data = [
        {
            id: 1,
            name: '🌧️',
            friendlyName: 'Heavy Rain',
            src: heavyRain,
            isPlaying: false,
            volume: 50
        },
        {
            id: 2,
            name: '🚗',
            friendlyName: 'Rain from a Car',
            src: rainFromACar,
            isPlaying: false,
            volume: 50
        },
        {
            id: 3,
            name: '🏖️',
            friendlyName: 'Rain on a Beach',
            src: rainBeach,
            isPlaying: false,
            volume: 50
        },
        {
            id: 4,
            name: '🌳',
            friendlyName: 'Rain in a Forest',
            src: rainForest,
            isPlaying: false,
            volume: 50
        }
    ];

    function toggleAudio(name) {
        const audio = document.getElementById(name);
        // Toggle paused property of audio element
        audio.paused ? audio.play() : audio.pause();

        // Update isPlaying property
        data = data.map(item => {
            if (item.name === name) {
                item.isPlaying = !item.isPlaying;
            }
            return item;
        });
    }

    function setVolume(name) {
        const audio = document.getElementById(name);
        const volume = event.target.value / 100;
        // Set volume property of audio element
        audio.volume = volume;
        // Update volume property of corresponding item in data array
        const item = data.find(item => item.name === name);
        item.volume = event.target.value;
    }

    function updateButtonLabels() {
        // Update button labels using Svelte's reactive declarations
        data.forEach(item => {
            item.label = item.isPlaying ? "Pause" : "Play";
        });
    }

    onMount(() => {
        // Call updateButtonLabels on mount and tick to ensure the DOM has updated
        updateButtonLabels();
        tick();
    });
</script>

<ul role="list" class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
    {#each data as item}
        <li class="col-span-1 flex flex-col divide-y divide-gray-200 rounded-lg bg-white text-center shadow {item.isPlaying ? 'shadow-purple-400 hover:shadow-purple-700' : ''}">
            <div class="flex flex-1 flex-col p-8">
                <div class="mx-auto h-32 w-32 flex-shrink-0 flex items-center justify-center">
                    <span class="text-8xl">{item.name}</span>
                </div>
                <audio src={item.src} id={item.name}></audio>
                <h3 class="mt-6 text-sm font-medium text-gray-900">{item.friendlyName}</h3>
                <dl class="mt-1 flex flex-grow flex-col justify-between">
                    <dd class="text-sm text-gray-500">Paradigm Representative</dd>
                    <dt class="sr-only">Role</dt>
                    <dd class="mt-3">
                        <label for="default-range" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                            🔊
                            <span class="rounded-full bg-green-100 px-2 py-1 text-xs font-medium text-green-800">{item.volume}%</span>
                        </label>
                        <input id="default-range" type="range"
                               class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700"
                               min="0" max="100" bind:value={item.volume} on:input={() => setVolume(item.name)}>
                    </dd>
                </dl>
            </div>
            <div>
                <div class="-mt-px flex divide-x divide-gray-200">
                    <div class="-ml-px flex w-0 flex-1">
                        <button on:click={() => toggleAudio(item.name)}
                                class="relative inline-flex w-0 flex-1 items-center justify-center gap-x-3 rounded-br-lg border border-transparent py-4 text-sm font-semibold text-gray-700 hove:text-gray-900 hover:font-bold">
                            {#if item.isPlaying}
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5" />
                                </svg>
                                <span>Pause</span>
                            {:else}
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                                    <path stroke-linecap="round" stroke-linejoin="round" d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.348a1.125 1.125 0 010 1.971l-11.54 6.347a1.125 1.125 0 01-1.667-.985V5.653z" />
                                </svg>
                                <span>Play</span>
                            {/if}
                        </button>
                    </div>
                </div>
            </div>
        </li>
    {/each}
</ul>
