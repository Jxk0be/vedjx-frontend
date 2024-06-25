<script>
    import HeroImage from "$lib/data/TN_theatre.jpg"
</script>

<div class="relative w-full max-w-[1500px] mx-auto h-screen flex items-center justify-center">
    <div class="absolute inset-0 bg-cover bg-center flex justify-center items-center"
         style={`background-image: url(${HeroImage});`}>
        <div class="absolute inset-0 bg-blue-700 bg-opacity-70"></div>
        <div class="relative z-10 text-center text-white">
            <h1 class="text-4xl md:text-6xl font-bold mb-4">Appalachian Web Designs</h1>
            <p class="text-lg md:text-xl">A Local Company</p>
        </div>
    </div>
</div>