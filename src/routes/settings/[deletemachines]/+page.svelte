<!-- <script>
	import { page } from "$app/stores";
	import { onMount } from "svelte";
    import Drawer from "$lib/drawer.svelte";


    let machinelist = [];
    let isLoading = true;
    let loading =false;

    let isDrawerOpen = false;

function toggleDrawer() {
    isDrawerOpen = !isDrawerOpen;
}

const fetchmachines = async () => {

       
       try {
           const response = await fetch(`https://telephone.http.vsensetech.in/admin/machines/${$page.params.deletemachines}`, {
               method: "GET",
           });

           if (response.ok) {
               const data = await response.json();
               machinelist = [...data.machines]; 
               console.log(machinelist);
               isLoading = false;
           } else {
               const errorData = await response.json();
               console.error("Error fetching machines:", errorData['message']);
           }
       } catch (error) {
           console.error("Error fetching machines:", error);
       }
   };
   onMount(fetchmachines)
</script> -->

<!-- <div class="relative mt-24 bg-white min-h-screen mb-8"> -->
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <!-- <button
    class="fixed top-1 left-4 p-4 text-2xl text-white rounded-xl transition duration-300 z-50"
    on:click={toggleDrawer}
>
    <i class="fas fa-bars"></i>
</button>

<Drawer {isDrawerOpen} {toggleDrawer} />

    {#if isLoading}
        <div class="fixed inset-0 flex items-center justify-center z-40">
            <div class="w-16 h-16 border-6 border-t-8 border-black border-solid rounded-full animate-spin"></div>
        </div>
        {:else}
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8 mt-8">
            {#each machinelist as machine}
                <div class="flex justify-center">
                    <div class="rounded-lg p-8 bg-white flex flex-col items-center justify-between h-80 w-80 space-y-4 border-2 border-gray-300 shadow-md hover:shadow-lg hover:scale-105 duration-200">
                        <span class="text-xl text-center font-bold text-gray-800">{machine.label}</span>
                        <span class="py-2 px-4 rounded-full text-xs font-semibold text-white bg-green-400 " >
                            <i class="fa-solid fa-cash-register mx-1"></i>   
                            {machine.machine_id.toUpperCase()}
                        </span>
                        <span class="text-3xl font-semibold text-gray-900 my-4">₹ {machine.balance.toLocaleString('en-IN')}</span>
                        <button class="text-base py-2 px-5 rounded-lg bg-gray-900 text-white font-medium "
                                on:click={()=>goto("/login/"+$page.params.listofmachines+"/"+machine.machine_id)}
                        >
                            Manage
                        </button>
                    </div>
                </div>
            {/each}  
        </div>
        
{/if}
</div> -->