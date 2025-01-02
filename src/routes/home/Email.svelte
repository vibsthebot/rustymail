<script lang="ts">
    import { invoke } from '@tauri-apps/api/tauri';
    export let uid = 0;
    let email: any = null;
    invoke<string>("fetch_email", { uid }).then(result => {
        email = JSON.parse(result);
        console.log(email);
    });
</script>

<div class="email-wrapper">
    {#if email}
        <div class="email">
            <div class="email-from">{email.from.replace(/<.*>/, '').trim()}</div>
            <div class="email-subject-preview">
                <div class="email-subject">{email.subject}</div>
                <div class="email-preview">{email.body}</div>
            </div>
            <p class="email-date">{email.date}</p>
            <div class="email-expanded">
                <div style="display: inline-flex; gap: 1rem; align-items: center; width: 100%;">
                    
                </div>
                <p class="expanded-body">{@html email.html_body}</p>
            </div>
        </div>
    {/if}
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap');

    :global(body) {
        font-family: 'Roboto', sans-serif;
    }
    .email-wrapper {
        margin: 0.5rem 0;
    }

    .email {
        display: grid;
        grid-template-columns: 200px 1fr 300px;
        gap: 1rem;
        align-items: center;
        border: 1px solid #e0e0e0;
        border-radius: 6px;
        padding: 0.75rem 2rem 0.75rem 1rem;
        transition: background-color 0.2s ease, padding 0.2s ease;
        position: relative;
    }

    .email-subject-preview {
        display: grid;
        grid-template-columns: 300px 1fr;
        gap: 1rem;
        align-items: center;
    }

    .email:hover {
        background-color: #f8f9fa;
    }

    .email-from {
        padding-left: 0.2rem;
    }
    
    .email-subject {
        font-weight: 500;
        padding-right: 10px;
    }

    .email-preview {
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        color: grey;
        font-size: 0.9rem;
    }

    .email-expanded {
        display: none;
        grid-column: 1 / -1;
        padding: 0rem 1rem;
        margin: 0;
        width: 100%;
        transition: display 2s;
    }

    .email:hover .email-expanded {
        display: block;
    }

    .email:hover .email-preview {
        display: none;
    }
    .email-date {
        text-align: right;
        font-size: 0.9rem; 
        color: grey;
    }
</style>