<script>
    import whatsappQr from '$lib/images/whatsapp-qr.png';
</script>

<a href="https://wa.me/+573233178235" target="_blank" class="ws-btn">
        <i class="fa-brands fa-whatsapp text-4xl text-white"></i>
        <div class="wsqr-btn">
            <h1 class="text-sm text-center font-bold mb-3">Linea Whatsapp</h1>
            <img class="w-32" src="{whatsappQr}" alt="">
        </div>
</a>

<style>
    .ws-btn {
        @apply transition-all fixed bottom-6 right-6 px-4 py-3 bg-[#25d366] hover:shadow-md rounded-full hover:rounded-bl-none hover:scale-105 cursor-pointer;
    }

    .ws-btn:hover .wsqr-btn {
        @apply opacity-100 p-4 h-auto transition-opacity;
    }

    .wsqr-btn {
        @apply bg-[#25d366] p-0 text-white rounded-xl rounded-br-none absolute bottom-0 -left-[270%] opacity-0 transition-opacity shadow-md overflow-hidden h-0;
    }
</style>