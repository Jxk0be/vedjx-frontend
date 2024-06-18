<script>
    import { PUBLIC_API_HOST } from '$env/static/public'
    import { goto } from '$app/navigation';
    import { browser } from "$app/environment"

    let success = false
    let usernameOrEmail = ""
    let password = ""

    const delay = (seconds) => {
        seconds = seconds * 1000
        return new Promise(resolve => setTimeout(resolve, seconds))
    }
    function setCookie(name, value, days) {
        let expires = "";
        if (days) {
            const date = new Date();
            date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
            expires = "; expires=" + date.toUTCString();
        }
        document.cookie = name + "=" + value + expires + "; path=/";
    }

    const apiLogin = async () => {
        let loginBody = {}

        /* Determining what we want to send to the login endpoint */
        if (usernameOrEmail.includes("@")) loginBody.email = usernameOrEmail
        else loginBody.username = usernameOrEmail
        loginBody.password = password

        try {
            const response = await fetch(`http://${PUBLIC_API_HOST}/api/user/login`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*"
                },
                body: JSON.stringify(loginBody)
            })

            const jsonResponse = await response.json()
            const user = jsonResponse?.user

            if (user) {
                if (browser) setCookie("accessToken", jsonResponse?.accessToken, 1)
                success = true
                await delay(1.5)
                goto("/")
            }
        }
        catch(error) {
            console.log(error)
        }
    }
</script>

{#if success}
    <h1>Success</h1>
{:else}
    <div class='w-full h-screen md:pt-[70px] pt-[60px] flex justify-center items-center'>
        <div class="bg-[#e2e0df] md:w-1/2 md:h-1/2 flex w-full h-full justify-center flex-col items-center text-[#4B4B4B] rounded-xl">
            <div class="flex justify-center items-center drop-shadow-md border-b-2 md:w-1/2 w-2/3 border-[#adb0ae]">
            <h1 class="text-[40px] font-bold">Login</h1>
            </div>
    
            <form class="mt-2 flex flex-col items-center w-2/3 md:w-1/2">
            <input bind:value={usernameOrEmail} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Username or Email" type="text" />
            <input bind:value={password} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Password" type="password" />
            <button class="mb-4 rounded-md w-full py-1 font-semibold bg-[#4B4B4B] duration-150 hover:bg-[#FF8200] hover:text text-white" type="submit" on:click={() => apiLogin()}>Login</button>
            </form>
            <h1 class="cursor-pointer text-slate-800 hover:text-[#FF8200] duration-150"><a href="/register" alt="register page">Don't have an account yet?</a></h1>
        </div>
    </div>
{/if}

