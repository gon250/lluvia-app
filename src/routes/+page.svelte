<script>
    import { onMount, onDestroy, tick } from 'svelte';

    import heavyRain from './sounds/heavy-rain.wav';
    import rainFromACar from './sounds/rain-from-a-car.wav';
    import rainBeach from './sounds/rain-beach.wav';
    import rainForest from './sounds/rain-forest.wav';

    const data = [
        {
            name: '🌧️',
            src: heavyRain,
            isPlaying: false
        },
        {
            name: '🚗',
            src: rainFromACar,
            isPlaying: false
        },
        {
            name: '🏖️',
            src: rainBeach,
            isPlaying: false
        },
        {
            name: '🌳',
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

<h1>Lluvia app</h1>
<div>
    {#each data as item}
        <div>
            <span>{item.name}</span>
            <audio src={item.src} id={item.name}></audio>
            <button on:click={() => toggleAudio(item.name)}>{#if item.isPlaying}Pause{:else}Play{/if}</button>
            <input type="range" min="0" max="100" value="50" on:input={() => setVolume(item.name)} />
        </div>
    {/each}
</div>
