<script>
   import { onMount } from "svelte";

        let isLoading = false;
        let inputText = "";
        let textPresent = false;
        let API_URL = "https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=";

        function dataSubmit(e) {
        e.preventDefault();
        if (inputText !== "") {
            textPresent = true;
            API_URL = `https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=${encodeURIComponent(inputText)}`;
            isLoading = true; 
        }
        }

       
        onMount(() => {
        isLoading = false;
        });

        function downloadQrCode() {
        fetch(API_URL)
        .then(response => response.blob())
        .then(blob => {
            const url = window.URL.createObjectURL(blob);

            const link = document.createElement('a');
            link.href = url;
            link.download = 'qrcode.png'; 

            document.body.appendChild(link);
            link.click();

            document.body.removeChild(link);

            
            window.URL.revokeObjectURL(url);
        });
    }
</script>
<div class="md:max-7xl flex justify-center mt-40 flex-col items-center">
    <div>
        <p class="text-4xl md:text-6xl font-bold font-mono m-auto">QR CODE <span class="font-mono text-green-600">GENERATOR</span></p>
    </div>
    <div>
        <p class="text-base md:text-2xl font-semibold font-mono mt-4 md:mt-3 m-auto">Created with 💚 by <span><a href="https://github.com/mwwlean" class=" text-green-500 hover:underline">@mwwlean</a></span> </p>
    </div>
</div>

<div class="md:max-7xl flex justify-center mt-10 flex-row">
    <div>
      <form on:submit={dataSubmit}>
        <input
          id="textInput"
          type="text"
          placeholder="Enter any text or URL..."
          bind:value={inputText}
          class="px-2 md:px-14 rounded-lg border-green-600 focus:border-green-600 border focus-within:border-green-600 focus-within:ring-1 focus-within:ring-green-600"
          required
        />
        <button
          id="btn"
          type="submit"
          class="ml-2 rounded-lg border border-current px-4 py-2.5 text-sm font-medium text-green-600 transition hover:rotate-2 hover:scale-110 ease-in-3 active:text-white hover:bg-green-600 hover:text-white font-mono "
        >
          Generate QR
        </button>
      </form>
      <div class="md:max-7xl flex justify-center mt-10 flex-col items-center rounded-xl ">
      {#if textPresent}
      {#if isLoading}
        <p class="font-mono text-green-600">Generating QR Code ...</p>
      {/if}
      
      <img
        class="w-60 object-cover h-full border p-5 border-green-200"
        src={API_URL}
        alt="QR Code"
        on:load={() => (isLoading = false)}
      />
      {#if !isLoading}
        <br />
        <button on:click={downloadQrCode} on:click={downloadQrCode} class="font-mono rounded-lg border border-current px-8 py-2.5 text-sm font-medium text-green-600 transition hover:rotate-2 hover:scale-110 ease-in-3 active:text-white hover:bg-green-600 hover:text-white"> 
          Download QR CODE
        </button>
      {/if}
    {/if}
   </div>
  </div>
 </div>
 

