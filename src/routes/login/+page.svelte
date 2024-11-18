<script lang="ts">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import Errormessage from "$lib/errormessage.svelte";
    import { page } from '$app/stores';
    import { get } from 'svelte/store';

    let vsenseText = '';
    const text = 'Vsense Technologies';
    const speed = 70;
    
    let loading = false;
    let errorMessage = '';
    let showError = false;
    let admin_name = '';
    let password = '';

    // Typing effect on mount
    onMount(() => {
        let i = 0;
        function typeEffect() {
            if (i < text.length) {
                vsenseText += text.charAt(i);
                i++;
                setTimeout(typeEffect, speed);
            }
        }
        typeEffect();
    });

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

<div class="min-h-screen flex items-center justify-center bg-white">
    <div class="w-full max-w-md bg-white rounded-2xl shadow-2xl p-6 flex flex-col justify-between relative">
        <div class="flex justify-center mb-4">
            <img class="h-24" src="logo.png" alt="vsenselogo" />
        </div>
        
        <div class="flex justify-center mb-6 text-2xl font-semibold text-gray-800">
            {vsenseText}
        </div>
        
        <form on:submit|preventDefault={handleLogin}>
            <div class="mb-6">
                <label class="block text-lg font-semibold mb-2" for="admin_name">Admin Name</label>
                <input
                    id="admin_name"
                    name="admin_name"
                    type="text"
                    placeholder="Email"
                    bind:value={admin_name}
                    class="w-full p-3 border border-gray-300 text-base md:text-lg focus:outline-none focus:border-black focus:ring-1 focus:ring-black rounded-2xl"
                    required
                />
            </div>
    
            <div class="mb-6">
                <label class="block text-lg font-semibold mb-2" for="password">Password</label>
                <input
                    id="password"
                    name="password"
                    type="password"
                    placeholder="Password"
                    bind:value={password}
                    class="w-full p-3 border border-gray-300 text-base md:text-lg focus:outline-none focus:border-black focus:ring-1 focus:ring-black rounded-2xl"
                    required
                />
            </div>

            <div class="flex justify-center mt-10">
                <button
                    type="submit"
                    class="w-full flex justify-center items-center bg-black text-white font-semibold py-3 rounded-xl text-lg focus:outline-none focus:ring-2 focus:ring-black focus:ring-offset-2"
                >
                    {#if loading}
                        <div class="animate-spin rounded-full h-6 w-6 border-t-4 border-white border-solid border-transparent"></div>
                    {:else}
                        Login
                    {/if}
                </button>
            </div>
        </form>

        {#if showError}
            <Errormessage errorMessage={errorMessage} />
        {/if}
    </div>
</div>
