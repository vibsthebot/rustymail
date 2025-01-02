<script lang="ts">
    import { invoke } from '@tauri-apps/api/tauri';
    import { goto } from '$app/navigation';
    
    let isLoading = false;
    let error = '';
    console.log('Login page loaded');

    async function handleGoogleLogin() {
        try {
            isLoading = true;
            error = '';
            const response = await invoke('generate_oauth_token', {});
            if (response) {
                invoke('validate_token').then((response) => {
                    if (response === true) {
                        goto('/home');
                    } else {
                        goto('/login');
                    }
                });
            }
        } catch (err) {
            error = 'Failed to login with Google. Please try again.';
            console.log('Error:', error);
            console.error('Error:', err);
        } finally {
            isLoading = false;
        }
    }
</script>

<div class="container">
    {#if error}
        <p class="error">{error}</p>
    {/if}
    
    <button 
        on:click={handleGoogleLogin} 
        disabled={isLoading}
        class="google-btn"
    >
        <img
            src="https://lh3.googleusercontent.com/COxitqgJr1sJnIDe8-jiKhxDx1FrYbtRHKJ9z_hELisAlapwE9LUPh6fcXIfb5vwpbMl4xl9H9TRFPc5NOO8Sb3VSgIBrfRYvW6cUA"
            alt="Google logo"
            class="google-icon"
        />
        {isLoading ? 'Connecting...' : 'Sign in with Google'}
    </button>
</div>

<style>
    .container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        gap: 1rem;
    }

    .google-btn {
        display: flex;
        align-items: center;
        gap: 1rem;
        padding: 0.75rem 1.5rem;
        background-color: white;
        color: #757575;
        border: 1px solid #dadce0;
        border-radius: 4px;
        font-family: 'Roboto', sans-serif;
        font-size: 14px;
        cursor: pointer;
        transition: background-color 0.2s;
    }

    .google-btn:hover {
        background-color: #f8f9fa;
    }

    .google-btn:disabled {
        opacity: 0.7;
        cursor: not-allowed;
    }

    .google-icon {
        width: 18px;
        height: 18px;
    }

    .error {
        color: #d93025;
        font-size: 14px;
    }
</style>