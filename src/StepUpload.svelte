<script lang="ts">
  import { Cloudinary } from "@cloudinary/url-gen";
  import { backgroundRemoval } from "@cloudinary/url-gen/actions/effect";

  import Dropzone from "dropzone";
  import { ImageStatus } from "../types.d";
  import { imageStatus, modifiedImage, originalImage } from "./store";
  import "dropzone/dist/dropzone.css";

  import { onMount } from "svelte";

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: "dwpiasf0z",
    },
    url: {
      secure: true,
    },
  });

  onMount(() => {
    const dropzone = new Dropzone("#dropzone", {
      uploadMultiple: false,
      acceptedFiles: ".jpg,.png,.webp ",
      maxFiles: 1,
    });

    dropzone.on("sending", (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING);
      formData.append("upload_preset", "lzregoss");
      formData.append("timestamp", Date.now() / 1000);
      formData.append("api_key", 138787377548912);
    });

    dropzone.on("success", (file, response) => {
      const { public_id: publicId, secure_url: secureUrl } = response;
      const imageWithoutBackground = cloudinary
        .image(publicId)
        .effect(backgroundRemoval());

      imageStatus.set(ImageStatus.DONE);
      originalImage.set(response.url);
      modifiedImage.set(imageWithoutBackground.toURL());

      console.log("success", response);
    });

    dropzone.on("error", (file, response) => {
      console.log("error", response);
    });
  });
</script>

<form
  id="dropzone"
  class="shadow-2xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col"
  action="https://api.cloudinary.com/v1_1/dwpiasf0z/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="pointer-events-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-4"
    >
      Upload Files
    </button>
    <strong class="text-lg mt-4"> or drop a file </strong>
  {/if}
  {#if $imageStatus === ImageStatus.UPLOADING}
    <strong class="text-lg mt-4"> uploading... </strong>
  {/if}
</form>
