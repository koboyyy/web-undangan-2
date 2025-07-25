<script>
  // Tentukan tanggal dan waktu acara (contoh: 31 Desember 2025, 23:59:59)
  const eventDate = new Date("Nov 31, 2025 23:59:59").getTime();
  let days = $state(0);
  let hours = $state(0);
  let minutes = $state(0);
  let seconds = $state(0);

  // Update countdown setiap 1 detik
  const countdown = setInterval(() => {
      const now = new Date().getTime();
      const distance = eventDate - now;

      // Hitung hari, jam, menit, detik
      days = Math.floor(distance / (1000 * 60 * 60 * 24));
      hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      seconds = Math.floor((distance % (1000 * 60)) / 1000);

      // Jika countdown selesai
      if (distance < 0) {
          clearInterval(countdown);
          days, hours, minutes, seconds = 0;
      }
  }, 1000);
</script>

<section>
  <div class="content">
      <div class="title">Save The Date</div>
      <div id="countdown">
        <div>
          <p>{days}</p>
          <p>days</p>
        </div>
        <div>
          <p>{hours}</p>
          <p>Jam</p>
        </div>
        <div>
          <p>{minutes}</p>
          <p>Menit</p>
        </div>
        <div>
          <p>{seconds}</p>
          <p>Detik</p>
        </div>
      </div>
  </div>
</section>

<style>
  .content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
    border-radius: 10px;
    backdrop-filter: blur(4px);
    background-color: rgba(255, 255, 255, 0.123);
    border: 1px solid #4e4e4e4b;
    color: #4E4E4E;
    padding: 10px;
  }

  .title {
    font-family: "BodoniModa", serif;
    font-size: 1.25em;
    color: #4E4E4E;
  }

  #countdown {
    display: flex;
    gap: 6px;
  }

  #countdown div {
    font-family: "Nunito-Regular", serif;
    letter-spacing: 1px;
    font-size: 0.7em;
    color: #4E4E4E;
    padding: 5px 14px;
    border-radius: 10px;
    background-color: rgba(8, 59, 35, 0.273);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>