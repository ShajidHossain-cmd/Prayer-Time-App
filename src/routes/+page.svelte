

<script>
 import { onMount } from 'svelte';

  let item = {};
  let latitude;
  let longitude;
  let locationObtained = false;
  let currentDate;

  async function fetchData(latitude, longitude, date) {
    try {
      const res = await fetch(
        `https://api.aladhan.com/v1/timings/${date}?latitude=${latitude}&longitude=${longitude}&method=1`
      );
      if (!res.ok) {
        throw new Error('Failed to fetch data from the Aladhan API');
      }
      item = await res.json();
    } catch (error) {
      console.error('Error fetching data from the Aladhan API:', error);
    }
  }

  onMount(() => {
    getLocation();
  });

  function getLocation() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        async position => {
          latitude = position.coords.latitude;
          longitude = position.coords.longitude;
          locationObtained = true; // Set to true when location is obtained
          const today = new Date();
          currentDate = `${today.getDate()}-${today.getMonth() + 1}-${today.getFullYear()}`;
          fetchData(latitude, longitude, currentDate); // Fetch data once location and date are obtained
        },
        error => {
          console.error('Error getting location:', error);
        }
      );
    } else {
      console.error('Geolocation is not supported by this browser.');
    }
  }

  function to12HourFormat(time) {
    const [hours, minutes] = time.split(':');
    const hour = parseInt(hours);
    const period = hour >= 12 ? 'PM' : 'AM';
    const hour12 = (hour % 12) || 12; // Convert 0 to 12 for midnight
    return `${hour12}:${minutes} ${period}`;
  }

</script>





 <main class="flex justify-center items-center min-h-screen">
  <div class="">
    
    <div class=" bg-center bg-[url('https://i.ibb.co/R7VMFdR/background.png')] absolute inset-0 flex justify-center items-center">
      <div class=" rounded-lg p-6">
        <h2 class="text-regal-yellow text-4xl font-serif text-center font-semibold mb-4">PRAYER</h2>

        {#if item.data}
        
        <div class= " p-4 flex items-center justify-between">
           <div class= "bg-regal-yellow p-4 flex w-40 items-center justify-between shadow-lg border-regal-blue ">
    <span class="font-semibold">SEHRI</span>
    <span>{to12HourFormat(item.data.timings.Fajr)}</span>
</div>
<div class="bg-regal-blue p-4 flex w-40 items-center justify-between shadow-lg border-regal-blue ">
              <span class="font-semibold uppercase">IFTAR</span>
              <span>{to12HourFormat(item.data.timings.Maghrib)}</span>
            </div>
        </div>
          <div class="flex flex-col text-xl gap-4 ">
           <div class= "bg-regal-yellow p-4 flex w-96 items-center justify-between shadow-lg border-regal-blue ">
    <span class="font-semibold">FAJIR</span>
    <span>{to12HourFormat(item.data.timings.Fajr)}</span>
</div>


            <div class="bg-regal-blue p-4 flex w-96 items-center justify-between shadow-lg border-regal-blue ">
              <span class="font-semibold uppercase">Dhuhr</span>
              <span>{to12HourFormat(item.data.timings.Dhuhr)}</span>
            </div>
            <div class="bg-regal-yellow p-4 flex w-96 items-center justify-between shadow-lg border-regal-blue ">
              <span class="font-semibold uppercase">Asr</span>
              <span>{to12HourFormat(item.data.timings.Asr)}</span>
            </div>
            <div class="bg-regal-blue p-4 flex w-96 items-center justify-between shadow-lg border-regal-blue ">
              <span class="font-semibold uppercase">Maghrib</span>
              <span>{to12HourFormat(item.data.timings.Maghrib)}</span>
            </div>
            <div class="bg-regal-yellow p-4 flex w-96 items-center justify-between shadow-lg border-regal-blue ">
              <span class="font-semibold uppercase">Isha</span>
              <span>{to12HourFormat(item.data.timings.Isha)}</span>
            </div>
          </div>
        {:else}
   
         <p class="text-4xl text-regal-yellow ">Fetching data...</p>
         
        {/if}
      </div>
    </div>
  </div>
</main> 