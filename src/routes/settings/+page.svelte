<!-- <script>
	import { goto } from "$app/navigation";
	import { onMount } from "svelte";
    import { page } from "$app/stores";

    let admin_name = '';
    let password = '';
    let loading = false;
    let errorMessage = '';
    let showError = false;
    
 

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
                goto("/settings/" + data.admin_id);
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

    onMount(handleLogin)
  </script>
  
  <div class="flex justify-center items-center min-h-screen bg-gray-50">
    
    <div class="w-full max-w-lg bg-white shadow-lg rounded-lg p-8">
  
  
      <div class="flex justify-center mb-6">
        <div class="w-16 h-16 bg-gray-200 rounded-full flex items-center justify-center">
          <img src="/logo.png" alt="Avatar" class="rounded-full" />
        </div>
      </div>
  
  
      <h1 class="text-2xl font-bold text-center mb-4">Account Confirmation</h1>
  
    
      <p class="text-gray-600 text-center mb-6">
        Please confirm your account by entering your <strong>admin name</strong> and <strong>password</strong> to proceed with deleting the machine.
      </p>
  
      <form on:submit|preventDefault={handleLogin}>
      <div class="space-y-4">
        <input
          type="text"
          placeholder="Admin Name"
          bind:value={admin_name}
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-black"
          required

        />
        <input
          type="password"
          placeholder="Password"
          bind:value={password}
          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-black"
          required
        />
      </div>

      <div class="flex justify-center mt-6">
        <button
          class="bg-black text-white font-medium py-2 px-4 rounded-lg hover:bg-gray-900"
          
          >
          Confirm and Proceed
        </button>
      </div>

      <p class="text-gray-400 text-xs text-center mt-6">
        If you are not authorized or did not intend to delete the machine, you can close this page.
      </p>
    </div>
  </div>
   -->