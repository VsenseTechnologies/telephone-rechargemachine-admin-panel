<script>
    import { goto } from "$app/navigation";
    import { page } from '$app/stores';
    import { onMount } from "svelte";
    import { writable } from "svelte/store";

    let machinelist = [];
    let showModal = false;
    let newMachineId = '';
    let newMachineLabel = ''; 
   

    const fetchmachines = async () => {
        try {
            const response = await fetch(`https://telephone.http.vsensetech.in/admin/machines/${$page.params.listofmachines}`, {
                method: "GET",
            });

            if (response.ok) {
                const data = await response.json();
                machinelist = [...data.machines]; 
                console.log(machinelist);
               
            } else {
                const errorData = await response.json();
                console.error("Error fetching machines:", errorData['message']);
            }
        } catch (error) {
            console.error("Error fetching machines:", error);
        }
    };



    const addMachines = async (event) => {
        event.preventDefault(); 
        
        try {
            const response = await fetch(`https://telephone.http.vsensetech.in/admin/create/machine/${$page.params.listofmachines}`, {
                method: "POST",
                body: JSON.stringify({ machine_id: newMachineId, label: newMachineLabel }),
               
            });

            if (response.ok) {
                newMachineId = '';
                newMachineLabel = ''; 
                showModal = false;
                await fetchmachines();
            } else {
                const errorData = await response.json();
                console.error("Error creating machine:", errorData["message"]);
            }
        } catch (error) {
            console.error("Error creating machine:", error);
        }
    };

    onMount(fetchmachines);
</script>

<div class="relative p-8 bg-white min-h-screen">
  
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8 mt-8">
        {#each machinelist as machine}
            <div class="flex justify-center">
                <div class="rounded-lg p-8 bg-white flex flex-col items-center justify-between h-80 w-80 space-y-6 border-2 border-gray-300 shadow-gray-300 shadow-sm duration-200 hover:shadow-lg hover:scale-105">
                    <span class="text-xl font-semibold text-black">{machine.label}</span>
                    <span class="text-4xl font-semibold text-black my-4">₹ {machine.balance}</span>
                    <button class="text-lg py-3 px-6 rounded-full bg-black text-white font-semibold"
                        on:click={() => goto("/login/"+$page.params.listofmachines+"/"+machine.machine_id)}>
                        Manage
                    </button>
                </div>
            </div>
        {/each}
    </div>
  
    <button class="fixed bottom-8 right-8 w-16 h-16 bg-black text-white text-4xl rounded-full flex items-center justify-center shadow-lg"
        on:click={() => showModal = true}>
        +
    </button>

    {#if showModal}
        <div class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50">
            <div class="bg-white p-8 rounded-lg shadow-lg w-96">
                <h2 class="text-xl font-semibold text-black mb-6 text-center justify-center py-4">Create New Machine</h2>
                <form on:submit={addMachines}>
                    <input type="text"
                           placeholder="Machine ID"
                           bind:value={newMachineId} 
                           class="w-full p-2 mb-10 border border-gray-300 rounded-lg text-lg" />

                    <input type="text" 
                           placeholder="Label" 
                           bind:value={newMachineLabel} 
                           class="w-full p-2 mb-10 border border-gray-300 rounded-lg text-lg" />

                    <div class="flex justify-between">
                        <button class="px-4 py-2 bg-white text-black rounded-lg text-lg border-2 border-gray-300 shadow-gray-300" 
                        on:click={() => showModal = false}>
                            Cancel
                        </button>
                        <button class="px-4 py-2 bg-black text-white rounded-lg text-lg">
                            Create
                        </button>
                    </div>
                </form>
            </div>
        </div>
    {/if}
</div>
