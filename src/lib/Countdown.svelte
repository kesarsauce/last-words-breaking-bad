<script>
    export let seconds = 60;
    setInterval(() => {
    if (seconds > 0) seconds--;
  }, 1000);
</script>

<div class="grid grid-flow-col gap-5 text-center auto-cols-max">
    <div class="flex flex-col p-2 bg-neutral rounded-box text-neutral-content">
      <span class="countdown font-mono text-5xl">
        <span style="--value:{seconds};"></span>
      </span>
    </div>
</div>