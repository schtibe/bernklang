<script>
  export let audioFile;

  import PlayButton from "./PlayButton.svelte";

  import { onDestroy } from "svelte";

  onDestroy(() => {});

  const SAMPLES = 70;
  const UPDATE_INTERVAL = 250;

  let dataBlocks = [];
  let audioElement;
  let domBlocksPlayed = [];
  let domBlocksHovered = [];
  let isPlaying = false;
  let isLoaded = false;

  let updateInterval;

  function filterData(audioBuffer) {
    const rawData = audioBuffer.getChannelData(0);
    const blockSize = Math.floor(rawData.length / SAMPLES);
    const filteredData = [];

    for (let i = 0; i < SAMPLES; i++) {
      let blockStart = blockSize * i;
      let sum = 0;
      for (let j = 0; j < blockSize; j++) {
        sum = sum + Math.abs(rawData[blockStart + j]);
      }
      filteredData.push(sum / blockSize);
    }
    return filteredData;
  }

  function normalizeData(filteredData) {
    const loudest = Math.max(...filteredData);
    const multiplier = Math.pow(loudest, -1);

    return filteredData.map((n) => n * multiplier);
  }

  const visualizeAudio = () => {
    isLoaded = true;
    fetch(audioFile)
      .then((response) => response.arrayBuffer())
      .then((arrayBuffer) => {
        window.AudioContext = window.AudioContext || window.webkitAudioContext;

        const audioContext = new AudioContext();
        return audioContext.decodeAudioData(arrayBuffer);
      })
      .then((audioBuffer) => {
        dataBlocks = normalizeData(filterData(audioBuffer));
      });
  };

  const lerp = (v0, v1, t) => {
    return v0 * (1 - t) + v1 * t;
  };

  function getHeight(block) {
    return lerp(0, 100, block);
  }

  function hoverUntil(lastBlockIndex) {
    const duration = audioElement.duration;
    const blockTime = (duration / SAMPLES) * lastBlockIndex;
    const currentTime = audioElement.currentTime;

    if (blockTime > currentTime) {
      for (let blockIndex = 0; blockIndex <= lastBlockIndex; blockIndex++) {
        domBlocksHovered[blockIndex] = true;
      }
    } else {
      const playedBlocks = domBlocksPlayed.filter((block) => block === true);

      for (
        let blockIndex = playedBlocks.length - 1;
        blockIndex >= lastBlockIndex;
        blockIndex--
      ) {
        domBlocksHovered[blockIndex] = true;
      }
    }
  }

  function clearHover() {
    for (const blockIndex in domBlocksHovered) {
      domBlocksHovered[blockIndex] = false;
    }
  }

  function updateVisualizer() {
    const position = audioElement.currentTime;
    const duration = audioElement.duration;

    for (let blockIndex in dataBlocks) {
      domBlocksPlayed[blockIndex] =
        position > (duration / SAMPLES) * blockIndex;
    }
  }

  function togglePlay() {
    if (isPlaying) {
      audioElement.pause();
    } else {
      audioElement.play();
    }
  }

  function seek(blockIndex) {
    const duration = audioElement.duration;
    const time = (duration / SAMPLES) * blockIndex;
    audioElement.currentTime = time;
    updateVisualizer();
  }

  function onPlay() {
    isPlaying = true;
    updateInterval = setInterval(() => {
      updateVisualizer();
    }, UPDATE_INTERVAL);
  }

  function onPause() {
    isPlaying = false;
    clearInterval(updateInterval);
  }

  function onEnded() {
    isPlaying = false;
    clearInterval(updateInterval);
  }

  //visualizeAudio(audioFile);
</script>

<div class="wrapper">
  <PlayButton {isPlaying} on:togglePlay={togglePlay} {isLoaded} />
  <div class="visualizer" id="visualizer">
    {#each dataBlocks as block, blockIndex}
      <div
        class="visualizerBlock"
        class:played={domBlocksPlayed[blockIndex]}
        class:hover={domBlocksHovered[blockIndex]}
        style="height: {getHeight(block)}%"
        on:mouseout={clearHover}
        on:mouseover={() => hoverUntil(blockIndex)}
        on:click={() => seek(blockIndex)}
        on:touchend={clearHover}
      >
        <div />
      </div>
    {/each}
  </div>
</div>

<audio
  id="audioSource"
  bind:this={audioElement}
  src={audioFile}
  on:play={onPlay}
  on:pause={onPause}
  on:ended={onEnded}
  on:canplay={visualizeAudio}
>
  <track kind="captions" />
</audio>

<style>
  .wrapper {
    display: flex;
  }

  .visualizer {
    margin-left: 5px;
    width: 100%;
    height: 75px;
    display: flex;
    align-items: center;
    justify-content: space-evenly;
  }

  .visualizerBlock {
    padding-left: 1px;
    flex-grow: 1;
    flex-shrink: 1;
    cursor: pointer;
  }

  .visualizerBlock > div {
    min-width: 1px;
    flex-grow: 1;
    background: rgb(255, 193, 59);
    height: 100%;
  }

  .visualizerBlock.played > div {
    background: rgb(241, 118, 17);
  }

  .visualizerBlock.hover > div {
    background: rgb(255, 163, 87);
  }
</style>
