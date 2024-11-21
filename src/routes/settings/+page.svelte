<script>
    import Drawer from "$lib/drawer.svelte";
    let isDrawerOpen = false;
    let loading =false;
    let showError = false;
    let errorMessage = '';

function toggleDrawer() {
    isDrawerOpen = !isDrawerOpen;
}

const handleLogin = async () => {
        loading = true;
        showError = false;
        errorMessage = '';

        try {
            const response = await fetch("https://telephone.http.vsensetech.in/login/admin", {
                method: "POST",
                body: JSON.stringify({ admin_name, password }),
            });

            if (response.ok) {
                const data = await response.json();
                console.log("Admin ID:", data.admin_id);             
                goto("/login/" + data.admin_id);
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
{#if loading}
<div class="fixed inset-0 flex items-center justify-center z-40">
    <div class="w-16 h-16 border-6 border-t-8 border-black border-solid rounded-full animate-spin"></div>
</div>
{/if}
</div>