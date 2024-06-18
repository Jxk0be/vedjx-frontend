<script>
    import { PUBLIC_API_HOST } from '$env/static/public'
    import { goto } from '$app/navigation';
    import { browser } from "$app/environment"

    let success = false
    let usernameOrEmail = ""
    let password = ""
    let resendEmail = ""
    let verifyEmailError = false
    let invalidEmail = false
    let validUser = false
    let user = {}
    let verEmailNotification = false
    let loginError = false
    let jsonResponse = null

    const clearFields = () => {
        usernameOrEmail = ""
        password = ""    
    }

    const isValidEmail = (email) => {
        if (email.length > 256) return false;
        
        let emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
        return emailRegex.test(email);
    }

    const delay = (seconds) => {
        seconds = seconds * 1000
        return new Promise(resolve => setTimeout(resolve, seconds))
    }

    const setCookie = (name, value, days) => {
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
            const response = await fetch(`${PUBLIC_API_HOST}/api/user/login`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*"
                },
                body: JSON.stringify(loginBody)
            })

            jsonResponse = await response.json()
            const user = jsonResponse?.user

            if (user) {
                if (browser) setCookie("accessToken", jsonResponse?.accessToken, 1)
                success = true
                await delay(1.5)
                goto("/")
            }
            else {
                loginError = true
                clearFields()
                await delay(2.25)
                loginError = false
            }

            if (response.status === 500) {
                verifyEmailError = true
                verEmailNotification = true
                await delay(2.25)
                verEmailNotification = false
            }
        }
        catch(error) {
            console.log(error)
        }
    }

    const resendVerification = async () => {
        if (!isValidEmail(resendEmail)) {
            invalidEmail = true
            resendEmail = ""
            await delay(2.25)
            invalidEmail = false
            return
        }

        try {
            const response = await fetch(`${PUBLIC_API_HOST}/api/user/resend-email`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*"
                },
                body: JSON.stringify({ email: resendEmail })
            })
            
            const jsonResponse = await response.json()
            user = jsonResponse?.user

            if (user) {
                validUser = true
                verifyEmailError = false
                await delay(2.25)
                validUser = false
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
    
            {#if verifyEmailError}
                <form class="mt-2 flex flex-col items-center w-2/3 md:w-1/2">
                    <input bind:value={resendEmail} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Username or Email" type="text" />
                    <button class="mb-4 rounded-md w-full py-1 font-semibold bg-[#FF8200] duration-150 hover:bg-[#4B4B4B] hover:text text-white" type="submit" on:click={() => resendVerification()}>Resend Verification Email</button>
                </form>
            {:else}
                <form class="mt-2 flex flex-col items-center w-2/3 md:w-1/2">
                    <input bind:value={usernameOrEmail} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Username or Email" type="text" />
                    <input bind:value={password} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Password" type="password" />
                    <button class="mb-4 rounded-md w-full py-1 font-semibold bg-[#4B4B4B] duration-150 hover:bg-[#FF8200] hover:text text-white" type="submit" on:click={() => apiLogin()}>Login</button>
                </form>
            {/if}
            <h1 class="cursor-pointer text-slate-800 hover:text-[#FF8200] duration-150"><a href="/register" alt="register page">Don't have an account yet?</a></h1>
        </div>
    </div>
{/if}

{#if invalidEmail}
    <div class={`absolute z-20 top-16 left-1/2 text-center transform -translate-x-1/2 bg-red-500 text-white px-4 py-2 rounded shadow-lg min-w-200 md:max-w-[600px] max-w-2/3 w-2/3`}>
        Error: Email is invalid, try again
    </div>
{/if}

{#if validUser}
    <div class={`absolute z-20 top-16 left-1/2 text-center transform -translate-x-1/2 bg-green-500 text-white px-4 py-2 rounded shadow-lg min-w-200 md:max-w-[600px] max-w-2/3 w-2/3`}>
        Sent to the email associated with {user?.username}
    </div>
{/if}

{#if verEmailNotification}
    <div class={`absolute z-20 top-16 left-1/2 transform -translate-x-1/2 text-center bg-red-500 text-white px-4 py-2 rounded shadow-lg min-w-200 md:max-w-[600px] max-w-2/3 w-2/3`}>
        Please verify your email first
    </div>
{/if}

{#if loginError}
    <div class={`${loginError ? '' : ''} absolute text-center z-20 top-16 left-1/2 transform -translate-x-1/2 bg-red-500 text-white px-4 py-2 rounded shadow-lg min-w-200 md:max-w-[600px] max-w-2/3 w-2/3`}>
        {jsonResponse}
    </div>
{/if}