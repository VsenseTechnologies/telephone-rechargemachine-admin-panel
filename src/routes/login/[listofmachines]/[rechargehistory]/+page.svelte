<script>
    import { page } from "$app/stores";
    import { onMount } from "svelte";
    import Drawer from "$lib/drawer.svelte";
    import Successmessage from "$lib/successmessage.svelte";
    import Errormessage from "$lib/errormessage.svelte";

    let activeTab = 'recharge';
    let rechargeData = [];
    let expenseData = [];
    let isModalOpen = false;
    let rechargeAmount = '';
    let loading = true;
    let isDrawerOpen = false;
    let successMessage = '';
    let errorMessage = '';
    let showError = false;
    let showModal = false;

    function toggleDrawer() {
        isDrawerOpen = !isDrawerOpen;
    }

    const fetchRechargeHistory = async () => {
        try {
            const response = await fetch(`https://telephone.http.vsensetech.in/admin/recharge/history/${$page.params.rechargehistory}`, {
                method: "GET"
            });
            if (response.ok) {
              
                const data = await response.json();
                console.log(data)
                rechargeData = data.recharge_history.map(recharge => {
                    const date = new Date(recharge.timestamp);
                    return {
                        ...recharge,
                        date: date.toLocaleDateString(),
                        time: date.toLocaleTimeString()
                    };
                });
            } else {
                const errorData = await response.json();
                console.error("Error fetching Recharge History:", errorData['message']);
            }
        } catch (error) {
            console.error("Error fetching Recharge History:", error);
        }
    };

    const fetchExpenseHistory = async () => {
        try {
            const response = await fetch(`https://telephone.http.vsensetech.in/admin/expense/history/${$page.params.rechargehistory}`, {
                method: "GET"
            });
            if (response.ok) {
                const data = await response.json();
                loading = false;
                expenseData = data.expense_history.map(expense => {
                    const date = new Date(expense.timestamp);
                    return {
                        ...expense,
                        date: date.toLocaleDateString(),
                        time: date.toLocaleTimeString()
                    };
                });
            } else {
                const errorData = await response.json();
                console.error("Error fetching Expense History:", errorData['message']);
            }
        } catch (error) {
            console.error("Error fetching Expense History:", error);
        }
    };

    onMount(() => {
        fetchRechargeHistory();
        fetchExpenseHistory();
    });

    function switchTab(tab) {
        activeTab = tab;
    }

    function openRechargeModal() {
        isModalOpen = true;
    }

    function closeRechargeModal() {
        isModalOpen = false;
        rechargeAmount = '';
    }

    async function handleRecharge() {
    if (rechargeAmount <= 0) {
        errorMessage = 'Please enter a valid recharge amount.'
        showError =true
        setTimeout(() => showError= '', 2000);
        return;
    }
    loading = true;
    showError = false;
    errorMessage = '';

    try {
        const response = await fetch(`https://telephone.http.vsensetech.in/admin/recharge/machine/${$page.params.rechargehistory}`, {
            method: "POST",
            body: JSON.stringify({ amount: parseFloat(rechargeAmount) })
        });

        if (response.ok) {
            console.log("Recharge successful!");
            successMessage = 'Recharged successfully!';
            showModal = false;
            setTimeout(() => successMessage = '', 2000);
            await fetchRechargeHistory();
            closeRechargeModal();
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
}

</script>

<div class="relative min-h-screen bg-white text-gray-800 font-sans" >
    <!-- svelte-ignore a11y_consider_explicit_label -->
    <button
        class="fixed top-1 left-4 p-4 text-2xl text-white rounded-xl transition duration-300 z-50"
        on:click={toggleDrawer}
    >
        <i class="fas fa-bars"></i>
    </button>

    <Drawer {isDrawerOpen} {toggleDrawer} />


    {#if loading}
    <div class="fixed inset-0 flex items-center justify-center z-40">
        <div class="w-16 h-16 border-6 border-t-8 border-black border-solid rounded-full animate-spin"></div>
    </div>
    {/if}

    <div class="flex justify-center mt-24 ">
        <!-- svelte-ignore a11y_click_events_have_key_events -->
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <div
            class={`px-6 py-2 text-lg font-semibold transition duration-200 cursor-pointer
                    ${activeTab === 'recharge' ? 'bg-gray-900 text-white shadow-md' : 'border-gray-300 border-2 text-gray-600 border-r-black'}`}
            on:click={() => switchTab('recharge')}
        >
            Recharge
        </div>

        <!-- svelte-ignore a11y_click_events_have_key_events -->
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <div
            class={`px-6 py-2 text-lg font-semibold transition duration-200 cursor-pointer
                    ${activeTab === 'expense' ? 'bg-gray-900 text-white shadow-md' : 'border-gray-300 border-2 text-gray-600 border-l-black'}`}
            on:click={() => switchTab('expense')}
        >
            Expense
        </div>
    </div>

    <div class="mt-8 px-6 mb-6">
        {#if activeTab === 'recharge'}
            <div class="overflow-x-auto rounded-lg border border-gray-200">
                <table class="w-full table-auto text-left">
                    <thead class="bg-gray-900 text-white">
                        <tr>
                            <th class="px-6 py-4 text-sm font-semibold text-center">ID</th>
                            <th class="px-6 py-4 text-sm font-semibold text-center">Amount</th>
                            <th class="px-6 py-4 text-sm font-semibold text-center">Date</th>
                            <th class="px-6 py-4 text-sm font-semibold text-center">Time</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each rechargeData as recharge, index}
                            <tr class="border-t hover:bg-gray-50 transition">
                                <td class="px-6 py-4 text-center">{index+1}</td>
                                <td class="px-6 py-4 text-center">{recharge.amount}</td>
                                <td class="px-6 py-4 text-center">{recharge.date}</td>
                                <td class="px-6 py-4 text-center">{recharge.time}</td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>

            <div class="fixed bottom-8 right-4">
               
                <!-- svelte-ignore a11y_consider_explicit_label -->
                <button class="fixed bottom-8 right-8 w-14 h-14 bg-black text-white text-xl rounded-full flex items-center justify-center shadow-lg"
                on:click={openRechargeModal}
                >
                <i class="fa-solid fa-indian-rupee-sign"></i>
                </button>
            </div>

        {:else if activeTab === 'expense'}
            <div class="overflow-x-auto  rounded-lg border border-gray-200">
                <table class="w-full table-auto text-left">
                    <thead class="bg-gray-900 text-white">
                        <tr>
                            <th class="px-6 py-4 text-sm font-semibold text-center">ID</th>
                            <th class="px-6 py-4 text-sm font-semibold text-center">Amount</th>
                            <th class="px-6 py-4 text-sm font-semibold text-center">Date</th>
                            <th class="px-6 py-4 text-sm font-semibold text-center">Time</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#each expenseData as expense, index}
                            <tr class="border-t hover:bg-gray-50 transition">
                                <td class="px-6 py-4 text-base text-center">{index+1}</td>
                                <td class="px-6 py-4 text-base text-center">{expense.amount}</td>
                                <td class="px-6 py-4 text-base text-center">{expense.date}</td>
                                <td class="px-6 py-4 text-base text-center">{expense.time}</td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        {/if}
    </div>

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
                        class="px-6 py-2  text-gray-700 rounded-lg shadow-sm transition duration-200 border-2 border-gray-300"
                        on:click={closeRechargeModal}
                    >
                        Cancel
                    </button>
                    <button class="px-4 py-2 bg-black text-white rounded-lg text-lg flex items-center justify-center min-w-[100px]"
                            on:click={handleRecharge}>
                        {#if loading}
                            <div class="animate-spin rounded-full h-6 w-6 border-t-4 border-white border-solid border-transparent"></div>
                        {:else}
                        Recharge
                        {/if}
                    </button>
                </div>
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

<style>
    /* Spinner animation */
    .animate-spin {
        animation: spin 1s linear infinite;
    }

    @keyframes spin {
        0% { transform: rotate(0deg); }
        100% { transform: rotate(360deg); }
    }
</style>
