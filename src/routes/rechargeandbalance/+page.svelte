<script>
    import Drawer from "$lib/drawer.svelte";
    let isDrawerOpen = false;
    let activeTab = 'recharge';
    let rechargeData = [
        { id: 1, amount: "500", date: "2024-11-16", time: "10:00:00" },
        { id: 2, amount: "300", date: "2024-11-15", time: "10:00:00" },
        { id: 3, amount: "700", date: "2024-11-14", time: "10:00:00" }
    ];
    let isModalOpen = false; // To control the modal visibility
    let rechargeAmount = ''; // Store the amount entered by the user

    function toggleDrawer() {
        isDrawerOpen = !isDrawerOpen;
    }

    function switchTab(tab) {
        activeTab = tab;
    }

    function openRechargeModal() {
        isModalOpen = true; // Open the modal
    }

    function closeRechargeModal() {
        isModalOpen = false; // Close the modal
    }

    function handleRecharge() {
        if (rechargeAmount) {
            // Handle the recharge logic here
            alert(`Recharged ₹${rechargeAmount}`);
            closeRechargeModal(); // Close modal after recharge
        } else {
            alert("Please enter a valid amount.");
        }
    }
</script>

<div class="relative min-h-screen bg-white text-gray-800 font-sans">
   
    <!-- Drawer Button -->
    <button
        class="fixed top-4 left-4 p-3 bg-gray-800 text-white rounded-full shadow-lg z-50"
        on:click={toggleDrawer}
    >
        <i class="fas fa-bars"></i>
    </button>
    <Drawer {isDrawerOpen} {toggleDrawer} />

    <!-- Tab Navigation -->
    <div class="flex justify-center mt-8 space-x-6">
        <button
            class={`px-8 py-4 text-xl font-semibold transition duration-200
                    ${activeTab === 'recharge' ? 'bg-gray-900 text-white shadow-md' : 'bg-gray-200 text-gray-600 hover:bg-gray-300'}`}
            on:click={() => switchTab('recharge')}
        >
            Recharge
        </button>
        <button
            class={`px-8 py-4 text-xl font-semibold transition duration-200
                    ${activeTab === 'expense' ? 'bg-gray-900 text-white shadow-md' : 'bg-gray-200 text-gray-600 hover:bg-gray-300'}`}
            on:click={() => switchTab('expense')}
        >
            Expense
        </button>
    </div>

    <!-- Tab Content -->
    <div class="mt-8 px-6">
        {#if activeTab === 'recharge'}
            <h2 class="text-4xl font-semibold mb-8 text-center text-gray-900">Recharge History</h2>
            <div class="overflow-x-auto shadow-xl rounded-lg border border-gray-200">
                <table class="w-full table-auto text-left">
                    <thead class="bg-gray-900 text-white">
                        <tr>
                            <th class="px-6 py-4 text-sm font-semibold text-left">ID</th>
                            <th class="px-6 py-4 text-sm font-semibold text-left">Amount</th>
                            <th class="px-6 py-4 text-sm font-semibold text-left">Date</th>
                            <th class="px-6 py-4 text-sm font-semibold text-left">Time</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each rechargeData as recharge}
                            <tr class="border-t hover:bg-gray-50 transition">
                                <td class="px-6 py-4 text-base">{recharge.id}</td>
                                <td class="px-6 py-4 text-base">{recharge.amount}</td>
                                <td class="px-6 py-4 text-base">{recharge.date}</td>
                                <td class="px-6 py-4 text-base">{recharge.time}</td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        {:else if activeTab === 'expense'}
            <h2 class="text-4xl font-semibold mb-8 text-center text-gray-900">Expense Details</h2>
            <div class="p-6 border rounded-lg bg-gray-50 shadow-md">
                <p class="text-lg text-gray-500">No expense data available yet.</p>
            </div>
        {/if}
    </div>

    <!-- Recharge Button at Bottom Right -->
    <button
        class="fixed bottom-6 right-6 px-6 py-4 text-xl font-semibold bg-gray-900 text-white rounded-lg shadow-lg transition duration-200 hover:bg-gray-800 z-50"
        on:click={openRechargeModal}
    >
        Recharge
    </button>

    <!-- Recharge Modal -->
    {#if isModalOpen}
        <div class="fixed inset-0 bg-gray-500 bg-opacity-50 flex items-center justify-center z-50">
            <div class="bg-white p-8 rounded-lg shadow-lg max-w-sm w-full">
                <h3 class="text-2xl font-semibold mb-4 text-center text-gray-900">Recharge Amount</h3>
                <div class="mb-6">
                    <label for="rechargeAmount" class="block text-sm font-medium text-gray-700">Enter Amount (₹)</label>
                    <input
                        id="rechargeAmount"
                        type="number"
                        min="1"
                        bind:value={rechargeAmount}
                        class="mt-2 p-2 w-full border border-gray-300 rounded-lg"
                        placeholder="Amount in ₹"
                    />
                </div>
                <div class="flex justify-between">
                    <button
                        class="px-6 py-2 bg-gray-900 text-white rounded-lg shadow-sm transition duration-200 hover:bg-gray-800"
                        on:click={handleRecharge}
                    >
                        Recharge
                    </button>
                    <button
                        class="px-6 py-2 bg-gray-200 text-gray-700 rounded-lg shadow-sm transition duration-200 hover:bg-gray-300"
                        on:click={closeRechargeModal}
                    >
                        Cancel
                    </button>
                </div>
            </div>
        </div>
    {/if}
</div>
