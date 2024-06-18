<script>
    import { PUBLIC_API_HOST } from '$env/static/public'
    import { goto } from '$app/navigation';

    let success = false
    $: registerError = false
    let username = ""
    let email = ""
    let password = ""
    $: confirmPass = ""
    let firstName = ""
    let lastName = ""
    let jsonResponse = null
    let passMatch = false

    const clearFields = () => {
        username = ""
        email = ""
        password = ""
        firstName = ""
        lastName = ""
        confirmPass = ""
    }

    const delay = (seconds) => {
        seconds = seconds * 1000
        return new Promise(resolve => setTimeout(resolve, seconds))
    }

    const apiRegister = async () => {
        let registerBody = {}

        if (!username || !email || !password || !firstName || !lastName) {
            console.log("Must submit all fields")
            return
        }

        const fullName = firstName + " " + lastName
        registerBody.username = username
        registerBody.email = email
        registerBody.password = password
        registerBody.fullName = fullName
        console.log(fullName)
        try {
            const response = await fetch(`${PUBLIC_API_HOST}/api/user/register`, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*"
                },
                body: JSON.stringify(registerBody)
            })
            
            jsonResponse = await response.json()
            const user = jsonResponse?.user
            
            if (user) {
                success = true
                clearFields()
                await delay(1.5)
                goto("/login")
            } else {
                registerError = true
                clearFields()
                await delay(2.25)
                registerError = false
            }
        }
        catch(error) {
            console.log(error)
        }
    }

    $: if (confirmPass === password && confirmPass !== "") {
        passMatch = true
       } else passMatch = false
</script>

{#if registerError}
    <div class={`${registerError ? '' : ''} absolute z-20 top-16 left-1/2 transform -translate-x-1/2 bg-red-500 text-white px-4 py-2 rounded shadow-lg min-w-200 md:max-w-[600px] max-w-2/3 w-2/3`}>
        {jsonResponse}
    </div>
{/if}

{#if success}
    <h1>Success</h1>
{:else}
    <div class='w-full h-screen md:pt-[70px] pt-[60px] flex justify-center items-center'>
        <div class="bg-[#e2e0df] md:w-1/2 md:h-1/2 flex md:min-w-[500px] w-full h-full justify-center flex-col items-center text-[#4B4B4B] rounded-xl">
            <div class="flex justify-center items-center drop-shadow-md md:min-w-[400px] border-b-2 md:w-1/2 w-2/3 border-[#adb0ae]">
                <h1 class="text-[40px] font-bold">Register</h1>
            </div>
    
            <form class="mt-2 flex flex-col items-center w-2/3 md:w-1/2 md:min-w-[400px]">
                <div class="flex flex-row w-full gap-x-3">
                    <input bind:value={firstName} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="First Name" type="text" />
                    <input bind:value={lastName} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Last Name" type="text" />
                </div>
                <input bind:value={username} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Username" type="text" />
                <input bind:value={email} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Email" type="text" />
                <input bind:value={password} class="outline-none placeholder-gray-500 placeholder-opacity-50 mb-4 w-full rounded-md pl-2 py-1 text-[20px]" placeholder="Password" type="password" />
                <input bind:value={confirmPass} class={`${confirmPass !== "" ? 'mb-0' : 'mb-4'} outline-none placeholder-gray-500 placeholder-opacity-50 w-full rounded-md pl-2 py-1 text-[20px]`} placeholder="Confirm Password" type="password" />
                
                {#if confirmPass !== ""}
                    <small id="passwordMatch" class={`${passMatch ? 'text-green-700' : 'text-red-600'} text-xs text-gray-500 mb-4`}>{passMatch ? 'Passwords match' : 'Passwords do not match'}</small>
                {/if}
                    <button class="mb-4 rounded-md w-full py-1 font-semibold bg-[#4B4B4B] duration-150 hover:bg-[#FF8200] hover:text text-white" type="submit" on:click={() => apiRegister()}>Register</button>
            </form>
            <h1 class="cursor-pointer text-slate-800 hover:text-[#FF8200] duration-150"><a href="/login" alt="register page">Already have an account?</a></h1>
        </div>
    </div>
{/if}
