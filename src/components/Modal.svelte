<script>
  import { createEventDispatcher } from "svelte";
  import { fade, fly } from "svelte/transition";
  const dispatch = createEventDispatcher();
</script>

<div class="modal-background" on:click={() => dispatch("close")} />

<div in:fly={{ y: 200, duration: 1000 }} out:fade class="entry-modal">
  <slot name="header" />
  <hr />
  <slot name="body" />
  <hr />
  <slot name="timestamp" />
  <hr />

  <button on:click={() => dispatch("close")}>close modal</button>
</div>

<style>
  .modal-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.3);
  }
  .entry-modal {
    position: absolute;
    left: 50%;
    top: 50%;
    width: calc(100vw - 4em);
    max-width: 32em;
    max-height: calc(100vh - 4em);
    overflow: auto;
    transform: translate(-50%, -50%);
    padding: 1em;
    border-radius: 0.2em;
    background: white;
  }
  button {
    display: block;
  }
</style>
