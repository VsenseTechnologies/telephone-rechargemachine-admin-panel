<script lang="ts">
    import { onMount } from 'svelte';

    let vsenseText = '';
    const text = 'Vsense Technologies';
    const speed = 70;

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
</script>

<div class="min-h-screen flex items-center justify-center bg-white">
    <div class="w-full max-w-md bg-white rounded-2xl shadow-2xl p-6 flex flex-col justify-between relative">
        <div class="flex justify-center mb-4">
            <img class="h-24" src="logo.png" alt="vsenselogo" />
        </div>
        
        <div class="flex justify-center mb-6 text-2xl font-semibold text-gray-800">
            {vsenseText}
        </div>
        
        <form>
            <div class="mb-6">
                <label class="block text-lg font-semibold mb-2" for="email">Email</label>
                <input
                    id="email"
                    name="email"
                    type="email"
                    placeholder="Email"
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
                    class="w-full p-3 border border-gray-300 text-base md:text-lg focus:outline-none focus:border-black focus:ring-1 focus:ring-black rounded-2xl"
                    required
                />
            </div>

            <div class="flex justify-center mt-10">
                <button
                    type="submit"
                    class="w-full flex justify-center items-center bg-black text-white font-semibold py-3 rounded-xl text-lg focus:outline-none focus:ring-2 focus:ring-black focus:ring-offset-2"
                >
                Login
                </button>
            </div>
        </form>
    </div>
</div>
