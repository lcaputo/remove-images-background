<script lang="ts">
  import { ImageStatus } from "../types.d";
  import CloudinaryLogo from "./CloudinaryLogo.svelte";
  import StepEdit from "./StepEdit.svelte";
  import StepUpload from "./StepUpload.svelte";
  import { imageStatus } from "./store";
</script>

<div id="content" class="max-w-xl w-full m-auto px-4">
  <header class="flex justify-center py-10">
    <h1 class="text-3xl font-bold text-blue-900 tracking-tighter">
      Remove <span class="text-blue-600">Background</span>
    </h1>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
    Powered by <a href="https://cloudinary.com/" target="_blank">
      <CloudinaryLogo />
    </a>
  </footer>
</div>

<style>
  :root {
    background-color: #f7fafc;
    color: #666666;
  }
  body {
    display: block !important;
  }
  #content {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>
