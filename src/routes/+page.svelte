<script>
    import { goto } from '$app/navigation';
    import { invoke } from '@tauri-apps/api/tauri'

    try {
        invoke('validate_token').then((response) => {
            if (response === true) {
                goto('/home');
            } else {
                goto('/login');
            }
            }).catch(() => {
                goto('/login');
            });
    } catch (error) {
        console.error('Navigation error:', error);
        goto('/login');
    }
    console.log('Page loaded');
</script>
