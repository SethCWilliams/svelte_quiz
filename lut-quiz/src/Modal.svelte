<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
</script>

<div transition:fade={{ x: 100 }} class="modal-bg">
  <div transition:fly={{ y: 100 }} class="modal">
    <button> Close </button>
    <slot />
    <!-- <slot>optional fallback</slot> -->
  </div>
</div>

<style>
  .modal-bg {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.8);
  }
  .modal {
    background: white;
    padding: 20px;
    border-radius: 15px;
  }
</style>
