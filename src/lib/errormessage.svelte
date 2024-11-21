<script>
    import { fade } from "svelte/transition";
    export let errorMessage;
</script>

<div class="fixed bottom-9 right-28 text-black bg-white border-2 border-gray-300 font-semibold p-4 rounded-md shadow-lg duration-300" transition:fade>
    <i class="fa-solid fa-circle-exclamation pr-2"></i>
    {errorMessage}
</div>