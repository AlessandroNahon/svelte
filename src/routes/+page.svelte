<script lang="ts">
  import { onMount } from 'svelte';

  let rotation = 0;
  let knobPosition = 0;

  const maxRotation = 180;
  const stepSize = maxRotation / 10;

  onMount(() => {
    const knob = document.getElementById('knob');
    knob.addEventListener('mousedown', handleMouseDown);
    window.addEventListener('mouseup', handleMouseUp);
    window.addEventListener('mousemove', handleMouseMove);
  });

  let isDragging = false;
  let previousMousePosition = 0;

  function handleMouseDown(event: MouseEvent) {
    isDragging = true;
    previousMousePosition = event.clientX;
  }

  function handleMouseUp() {
    isDragging = false;
  }

  function handleMouseMove(event: MouseEvent) {
    if (isDragging) {
      const delta = event.clientX - previousMousePosition;
      previousMousePosition = event.clientX;

      knobPosition += delta;
      rotation = Math.max(0, Math.min(maxRotation, rotation + delta));
    }
  }
</script>

<style>
  .knob {
    width: 200px;
    height: 200px;
    margin: 100px;
    position: relative;
    cursor: pointer;
  }

  .dial {
    width: 100%;
    height: 100%;
    position: absolute;
    border-radius: 50%;
    background-color: #eee;
    transform: rotate(0deg);
    transition: transform 0.2s ease;
  }

  .indicator {
    width: 0;
    height: 0;
    border-left: 5px solid transparent;
    border-right: 5px solid transparent;
    border-bottom: 15px solid #333;
    position: absolute;
    top: 5px;
    left: calc(50% - 5px);
  }

  .steps {
    position: absolute;
    top: 45%;
    left: calc(100% - 3px);
    transform: translate(-50%, -50%) rotate(-5deg);
    display: flex;
    flex-direction: row;
    align-items: center;
    width: 100%;
    height: 100%;
  }

  .step {
    font-size: 12px;
    position: absolute;
    top: 50%;
    transform-origin: center center;
    user-select: none;
  }
</style>

<div class="knob" id="knob">
  <div class="dial" style={`transform: rotate(${rotation - 90}deg)`}>
    <div class="indicator"></div>
  </div>
  <div class="steps">
    {#each Array.from({ length: 10 }, (_, i) => i + 1) as step}
      <div
        class="step"
        style={`transform: rotate(${step * stepSize - 3}deg) translate(-112px) rotate(-${step * stepSize - 6}deg`}
      >
        {step}
      </div>
    {/each}
  </div>
</div>
