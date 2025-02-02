// Fungsi untuk membuat halaman video baru
function createVideoPage(videoFile, title) {
    // Membuat nama file video
    const videoName = title.replace(/\s+/g, '-').toLowerCase();

    // Membuat URL halaman baru
    const videoPageUrl = `${videoName}.html`;

    // Simulasi pembuatan halaman video baru (misalnya menggunakan server)
    const videoData = {
        title: title,
        videoFile: URL.createObjectURL(videoFile),
        thumbnail: generateThumbnail(videoFile)
    };

    // Menyimpan data video di localStorage
    localStorage.setItem(videoPageUrl, JSON.stringify(videoData));

    alert('Video berhasil diunggah! Kunjungi halaman video: ' + videoPageUrl);
    window.location.href = 'index.html'; // Redirect ke halaman utama setelah upload
}

// Fungsi untuk menghasilkan thumbnail video
function generateThumbnail(file) {
    const videoElement = document.createElement('video');
    videoElement.src = URL.createObjectURL(file);
    videoElement.load();

    return new Promise((resolve) => {
        videoElement.onloadeddata = () => {
            const canvas = document.createElement('canvas');
            const ctx = canvas.getContext('2d');
            ctx.drawImage(videoElement, 0, 0, canvas.width, canvas.height);
            resolve(canvas.toDataURL());
        };
    });
}

// Mengelola pengunggahan video
document.getElementById('uploadForm').addEventListener('submit', function(e) {
    e.preventDefault();

    const videoFile = document.getElementById('videoFile').files[0];
    const videoTitle = document.getElementById('videoTitle').value;

    if (videoFile && videoTitle) {
        createVideoPage(videoFile, videoTitle);
    } else {
        alert('Silakan lengkapi semua data!');
    }
});

// Menampilkan daftar video di halaman utama
window.onload = function() {
    const videosContainer = document.getElementById('videos');
    videosContainer.innerHTML = ''; // Bersihkan video sebelumnya

    for (let i = 0; i < localStorage.length; i++) {
        const videoPageUrl = localStorage.key(i);
        const videoData = JSON.parse(localStorage.getItem(videoPageUrl));

        const videoDiv = document.createElement('div');
        videoDiv.classList.add('video-item');

        const videoThumbnail = document.createElement('img');
        videoThumbnail.src = videoData.thumbnail;
        videoThumbnail.classList.add('video-thumbnail');
        
        const videoLink = document.createElement('a');
        videoLink.href = videoPageUrl;
        videoLink.innerText = videoData.title;

        videoDiv.appendChild(videoThumbnail);
        videoDiv.appendChild(videoLink);

        videosContainer.appendChild(videoDiv);
    }

    function generateThumbnail(file) {
        return new Promise((resolve, reject) => {
          const videoElement = document.createElement('video');
          videoElement.src = URL.createObjectURL(file);
          videoElement.muted = true; // Diperlukan agar browser dapat memuat video tanpa interaksi
          videoElement.playsInline = true; // Agar berjalan dengan baik di perangkat mobile
      
          // Pastikan video tidak ditampilkan
          videoElement.style.display = 'none';
          document.body.appendChild(videoElement);
      
          // Saat metadata video sudah dimuat, kita dapat mengambil durasi
          videoElement.addEventListener('loadedmetadata', () => {
            const duration = videoElement.duration;
            // Pilih detik acak dalam durasi video
            const randomTime = Math.random() * duration;
            videoElement.currentTime = randomTime;
          });
      
          // Saat video sudah berpindah ke detik acak, ambil frame sebagai thumbnail
          videoElement.addEventListener('seeked', () => {
            const canvas = document.createElement('canvas');
            // Gunakan lebar dan tinggi asli video
            canvas.width = videoElement.videoWidth;
            canvas.height = videoElement.videoHeight;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(videoElement, 0, 0, canvas.width, canvas.height);
            const thumbnailData = canvas.toDataURL('image/png');
      
            // Hapus elemen video yang sudah tidak diperlukan
            document.body.removeChild(videoElement);
      
            resolve(thumbnailData);
          });
      
          videoElement.addEventListener('error', (e) => {
            document.body.removeChild(videoElement);
            reject("Error generating thumbnail");
          });
        });
      }
     
      
         
        
};
