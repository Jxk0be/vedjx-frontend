<script>
    import { onMount } from "svelte";
    import { PUBLIC_API_HOST } from '$env/static/public'
    import { page } from '$app/stores'
    import { goto } from '$app/navigation';

    let response = null

    const delay = (seconds) => {
        seconds = seconds * 1000
        return new Promise(resolve => setTimeout(resolve, seconds))
    }

    onMount(async () => {
        try {
            const emailToken = $page.url.searchParams.get('emailToken')
            
            response = await fetch(`http://${PUBLIC_API_HOST}/api/user/verify-email`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ 
                    emailToken: emailToken 
                })
            })

            await delay(2.75)
            if (response?.status === 200) goto("/")
        }
        catch(error) {
            console.log(error)
        }
    })
</script>

{#if response?.status === 400}
    <h1 class="font-bold text-red-400 text-[100px]">Failed</h1>
{:else if response?.status === 200}
    <h1 class="font-bold text-red-400 text-[100px]">Success</h1>
{:else}
    <h1 class="font-bold text-red-400 text-[100px]">Verifying</h1>
{/if}