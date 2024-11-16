<script>
   export let isDrawerOpen = false;

function toggleDrawer() {
        isDrawerOpen = !isDrawerOpen;
    }
</script>


{#if isDrawerOpen}

    <!-- svelte-ignore a11y_click_events_have_key_events -->
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div
        class="fixed inset-0 bg-black bg-opacity-50 z-40"
        on:click={toggleDrawer} 
    ></div>

    <div
        class="fixed inset-y-0 left-0 w-60 bg-white shadow-xl z-50 transform transition-transform duration-300 ease-in-out rounded-r-2xl"
    >
      

            <ul class="flex flex-col flex-grow space-y-2 py-4">
                <li>
                    <button
                        class="flex items-center text-lg text-black text-left rounded-lg hover:text-white hover:bg-black duration-300 py-4 px-5 font-semibold w-full"
                        on:click={() => {goto("/machines");isDrawerOpen=false;}}
                    >
                        <i class="fa-solid fa-home mr-3"></i> Home
                    </button>
                </li>
                <li>
                    <button
                        class="flex items-center text-lg text-black text-left rounded-lg hover:text-white hover:bg-black duration-300 py-4 px-5 font-semibold w-full"
                        on:click={() => goto("/giveaccess")}
                    >
                        <i class="fa fa-users mr-3"></i> Access
                    </button>
                </li>
                <li>
                    <button
                        class="flex items-center text-lg text-black text-left rounded-lg hover:text-white hover:bg-black duration-300 py-4 px-5 font-semibold w-full"
                        on:click={() => {goto("/settings");isDrawerOpen=false;}}
                    >
                        <i class="fa-solid fa-cog mr-3"></i> Settings
                    </button>
                </li>
            </ul>

            <div class="mt-auto pb-4">
                <button
                    class="flex items-center text-lg text-black text-left rounded-lg hover:text-white hover:bg-black    duration-300 py-4 px-5 font-semibold w-full"
                    on:click={() => goto("/login")}
                >
                    <i class="fa-solid fa-sign-out-alt mr-3"></i> Logout
                </button>
            </div>
        </div>

{/if}

<!-- svelte-ignore a11y_consider_explicit_label -->
<button on:click={toggleDrawer} class="fixed bottom-4 right-4 p-4 ">
   
</button>