<script>
    import { goto } from "$app/navigation";
    import { page } from '$app/stores';
    import { onMount } from "svelte";
    import { writable } from "svelte/store";
    import Drawer from "$lib/drawer.svelte";
    import Successmessage from "$lib/successmessage.svelte";
    import Errormessage from "$lib/errormessage.svelte";

    let isDrawerOpen = false;

    function toggleDrawer() {
        isDrawerOpen = !isDrawerOpen;
    }

    let machinelist = [];
    let showModal = false;
    let newMachineId = '';
    let newMachineLabel = ''; 
    let isLoading = true;
    let loading =false;
    let successMessage = '';
    let errorMessage = '';
    let showError = false;

    const fetchmachines = async () => {
       
        try {
            const response = await fetch(`https://telephone.http.vsensetech.in/admin/machines/${$page.params.listofmachines}`, {
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

    const addMachines = async (event) => {
    showError = false;
    errorMessage = '';
    event.preventDefault();
    loading = true;
    try {
        const response = await fetch(`https://telephone.http.vsensetech.in/admin/create/machine/${$page.params.listofmachines}`, {
            method: "POST",
            body: JSON.stringify({ machine_id: newMachineId, label: newMachineLabel }),
        });
        if (response.ok) {
            newMachineId = '';
            newMachineLabel = '';
            showModal = false;
            successMessage = 'Machine created successfully!';
            setTimeout(() => successMessage = '', 2000);
            await fetchmachines();
        } else {
            const jsonResponse = await response.json();
                errorMessage = jsonResponse.message || 'An unexpected error occurred.';
                showError = true;
        }
    } catch (error) {
        console.error("Error:", error);
            errorMessage = 'Failed to communicate with the server. Please try again later.';
            showError = true;
    } finally {
        loading = false;
            setTimeout(() => {
                showError = false;
            }, 3000);
    }
};


    onMount(fetchmachines);
</script>

<div class="relative mt-24 bg-white min-h-screen mb-8">
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <button
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
                        <span class="py-2 px-4 rounded-full text-xs font-semibold text-white bg-green-400" >{machine.machine_id.toUpperCase()}</span>
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
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <button class="fixed bottom-8 right-8 w-14 h-14 bg-black text-white text-xl rounded-full flex items-center justify-center shadow-lg"
        on:click={() => showModal = true}>
        <i class="fa-solid fa-plus"></i>
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
                        <button class="px-4 py-2 bg-black text-white rounded-lg text-lg flex items-center justify-center min-w-[100px]">
                            {#if loading}
                                <div class="animate-spin rounded-full h-6 w-6 border-t-4 border-white border-solid border-transparent"></div>
                            {:else}
                                <span>Create</span>
                            {/if}
                        </button>
                        
                    </div>
                </form>
            </div>
        </div>
    {/if}
    {#if successMessage}
    <Successmessage successMessage={successMessage}/>
{/if}
{#if showError}
<Errormessage errorMessage={errorMessage} />
{/if}
</div>
