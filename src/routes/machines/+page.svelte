<script>
	import { goto } from "$app/navigation";
    import Drawer from "$lib/drawer.svelte";
    let isDrawerOpen = false;
    const machines = [
        { name: "Machine A", price: "₹ 1,200.00" },
        { name: "Machine B", price: "₹ 1,500.00" },
        { name: "Machine C", price: "₹ 2,000.00" },
        { name: "Machine D", price: "₹ 2,500.00" },
        { name: "Shashi", price: "₹ 3,500.00" }
    ];

    function toggleDrawer() {
        isDrawerOpen = !isDrawerOpen;
    }
</script>

<div class="relative">
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <button
        class="fixed top-4 left-4 py-0 text-2xl text-white rounded-xl transition duration-300 z-50"
        on:click={toggleDrawer}
    >
        <i class="fas fa-bars"></i>
    </button>
    <Drawer {isDrawerOpen} {toggleDrawer} />
    
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8 mt-8">
        {#each machines as machine}
            <div class="flex justify-around">
                <div class="rounded-lg p-6 bg-black flex flex-col items-center justify-between h-72 w-72 shadow-lg hover:shadow-xl">
                    <span class="text-2xl font-semibold text-gray-200">{machine.name}</span> 
                    <span class="text-3xl font-bold text-white my-4">{machine.price}</span>
                    <button class="text-xl p-3 rounded-xl bg-white font-bold shadow-md"
                    on:click={()=>goto("/rechargeandbalance")}
                    >
                        Manage
                    </button>
                </div>
            </div>
        {/each}
    </div>
</div>
