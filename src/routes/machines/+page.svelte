<script>
  
    const machines = [
        { name: "Machine A", price: "₹ 1,200.00" },
        { name: "Machine B", price: "₹ 1,500.00" },
        { name: "Machine C", price: "₹ 2,000.00" },
        { name: "Machine D", price: "₹ 2,500.00" }
    ];
</script>

<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mt-8">
 
    {#each machines as machine}
        <div class="p-4">
            <div class="rounded-lg p-6 bg-black flex flex-col items-center justify-between h-72 w-72 shadow-lg hover:shadow-xl transition-transform duration-300 transform hover:scale-105">
                <span class="text-2xl font-semibold text-gray-200">{machine.name}</span> 
                <span class="text-3xl font-bold text-white my-4">{machine.price}</span>
                <button class="text-xl p-3 rounded-xl bg-white font-bold shadow-md">
                    Manage
                </button>
            </div>
        </div>
    {/each}
</div>      
