<script>
    import { goto } from "$app/navigation";
    import { page } from "$app/stores";

    export let isDrawerOpen = false;

    function toggleDrawer() {
        isDrawerOpen = !isDrawerOpen;
    }

    function handleKeyPress(event) {
        if (event.key === "Escape") {
            isDrawerOpen = false;
        }
    }

    const buttonClass = "flex items-center text-lg text-black text-left rounded-lg border-2 border-bg-gray-300 py-4 px-5 font-semibold w-full";
</script>


{#if isDrawerOpen}
    <!-- svelte-ignore a11y_no_noninteractive_tabindex -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div
        class="fixed inset-0 bg-black bg-opacity-50 z-40 transition-opacity duration-300 ease-in-out opacity-0 animate-fade-in"
        on:click={toggleDrawer}
        on:keydown={handleKeyPress}
        tabindex="0"
    ></div>

    
    <div
        class="fixed inset-y-0 left-0 w-60 bg-white shadow-xl z-50 transform transition-transform duration-300 ease-in-out rounded-r-2xl -translate-x-full animate-slide-in"
        aria-hidden={!isDrawerOpen}
        role="dialog"
    >
        <div class="px-4 flex flex-col h-full">
            <ul class="flex flex-col flex-grow space-y-2 py-4">
                <li>
                    <div class="flex justify-center mb-4">
                        <img class="h-32" src="../../logo.png" alt="vsenselogo" />
                    </div>
                    <button
                        class={buttonClass}
                        on:click={() => {goto("/login/"+$page.params.listofmachines); isDrawerOpen = false;}}
                    >
                        <i class="fa-solid fa-home mr-3"></i> Home
                    </button>
                </li>
            
                
                <li class="my-6"></li>
            
                <li>
                    <button
                        class={buttonClass}
                        on:click={() => {goto("/settings"); isDrawerOpen = false;}}
                    >
                        <i class="fa-solid fa-cog mr-3"></i> Settings
                    </button>
                </li>
            </ul>
            
            <div class="mt-auto pb-4">
                <button
                    class={buttonClass}
                    on:click={() => {goto("/login"); isDrawerOpen = false;}}
                >
                    <i class="fa-solid fa-sign-out-alt mr-3"></i> Logout
                </button>
            </div>
        </div>
    </div>
{/if}

<style>

    @keyframes fade-in {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }

    @keyframes slide-in {
        from {
            transform: translateX(-100%);
        }
        to {
            transform: translateX(0);
        }
    }

    .animate-fade-in {
        animation: fade-in 0.3s forwards;
    }

    .animate-slide-in {
        animation: slide-in 0.3s forwards;
    }
</style>
