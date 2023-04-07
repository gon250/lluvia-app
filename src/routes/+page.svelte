<script>
    import { onMount, onDestroy, tick } from 'svelte';

    import heavyRain from './sounds/heavy-rain.wav';
    import rainFromACar from './sounds/rain-from-a-car.wav';
    import rainBeach from './sounds/rain-beach.wav';
    import rainForest from './sounds/rain-forest.wav';

    const data = [
        {
            name: '🌧️',
            friendlyName: 'Heavy Rain',
            src: heavyRain,
            isPlaying: false
        },
        {
            name: '🚗',
            friendlyName: 'Rain from a Car',
            src: rainFromACar,
            isPlaying: false
        },
        {
            name: '🏖️',
            friendlyName: 'Rain on a Beach',
            src: rainBeach,
            isPlaying: false
        },
        {
            name: '🌳',
            friendlyName: 'Rain in a Forest',
            src: rainForest,
            isPlaying: false
        }
    ];

    function toggleAudio(name) {
        const audio = document.getElementById(name);
        const item = data.find(item => item.name === name);
        // Toggle paused property of audio element
        audio.paused ? audio.play() : audio.pause();
        // Update isPlaying property
        item.isPlaying = !item.isPlaying;
    }

    function setVolume(name) {
        const audio = document.getElementById(name);
        const volume = event.target.value / 100;
        // Set volume property of audio element
        audio.volume = volume;
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

    onDestroy(() => {
        // Cleanup code
    });

    export { data };
</script>

<ul role="list" class="grid grid-cols-1 gap-6 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4">
    {#each data as item}
        <li class="col-span-1 flex flex-col divide-y divide-gray-200 rounded-lg bg-white text-center shadow">
            <div class="flex flex-1 flex-col p-8">
                <div class="mx-auto h-32 w-32 flex-shrink-0 flex items-center justify-center">
                    <span class="text-8xl">{item.name}</span>
                </div>
                <h3 class="mt-6 text-sm font-medium text-gray-900">{item.friendlyName}</h3>
                <dl class="mt-1 flex flex-grow flex-col justify-between">
                    <dd class="text-sm text-gray-500">Paradigm Representative</dd>
                    <dt class="sr-only">Role</dt>
                    <dd class="mt-3">
                        <label for="default-range" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                            🔊
                            <span class="rounded-full bg-green-100 px-2 py-1 text-xs font-medium text-green-800">50%</span>
                        </label>
                        <input id="default-range" type="range"
                               class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700"
                               min="0" max="100" value="50" on:input={() => setVolume(item.name)}>
                    </dd>
                </dl>
            </div>
            <div>
                <div class="-mt-px flex divide-x divide-gray-200">
                    <div class="-ml-px flex w-0 flex-1">
                        <button on:click={() => toggleAudio(item.name)}
                           class="relative inline-flex w-0 flex-1 items-center justify-center gap-x-3 rounded-br-lg border border-transparent py-4 text-sm font-semibold text-gray-900">
                            <svg class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor"
                                 aria-hidden="true">
                                <path d="M21 7.5V18M15 7.5V18M3 16.811V8.69c0-.864.933-1.406 1.683-.977l7.108 4.061a1.125 1.125 0 010 1.954l-7.108 4.061A1.125 1.125 0 013 16.811z"
                                      stroke-linecap="round" stroke-linejoin="round"></path>
                            </svg>
                            {#if item.isPlaying}Pause{:else}Play{/if}
                        </button>
                    </div>
                </div>
            </div>
        </li>
    {/each}
</ul>
