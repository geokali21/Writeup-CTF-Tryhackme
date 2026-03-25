<h1>Writeup-TakeOver</h1>
<ol>
    Point Penting!.
    <li>Room ini berfokus pada subdomain enumeration.</li>
    <li>Tambahkan <code>futurevera.thm</code> di <code>/etc/hosts</code></li>
</ol>
<strong>Pada write-up ini, saya akan menjelaskan seluruh langkah yang saya lakukan untuk menyelesaikan challenge, baik yang berhasil maupun yang tidak, sebagai bagian dari proses pembelajaran.</strong>

<h3>Analisis Awal (Web Enumeration)</h3>
<ul>
    <strong>Hal yang ditemukan :-</strong> 
    <li>Mengalami SSL error saat mengakses website</li>
    <li>Tidak ada direktori yang menarik</li>
    <li>Tidak ada informasi penting pada source code</li>
    <li>Tidak ada informasi berguna pada file <code>.js</code> </li>
    <li>Tidak ada informasi signifikan pada <code> Certificate </code></li>
</ul>

<h3>Fuzzing Subdomain dengan ffuf.</h3>
<code>ffuf -u http://{MachineIp}/ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words.txt -H 'Host: FUZZ.futurevera.thm' -fs {size}</code>
<ul>
    <li>Ditemukan dua subdomain Namun keduanya menampilkan pesan:<br><code>{subdomain}.futurevera.thm is only availiable via internal VPN</code>
    <li>Tidak ditemukan informasi penting saat melakukan enumerasi direktori atau file</li>
</ul>
</code></p>
<p>Setelah mencoba berbagai wordlist tanpa hasil signifikan, saya mengubah pendekatan dengan menggunakan <code>https</code> Ditemukan dua subdomain baru</p>
<h3>Enumerating subdomains baru</h3>
<ul>
    <li>Tetap tidak ada hasil dari directory enumeration.</li>
    <li>Namun ditemukan sesuatu yang menarik pada bagian <code>view certificate</code>.</li>
</ul>
<h3>Flag</h3>
<p>Perhatikan sertifikat untuk kedua subdomain tersebut dan Anda akan melihat satu subdomain lagi di <br><code>Subject Alt Names</code></p>
<p>Cara melihat certificate (Firefox):</p>
<ol>
    <li>Klik ikon 🔒 pada address bar (biasanya dengan tanda peringatan)</li>
    <li>Click pada <code>Connection not scure</code></li>
    <li>Click pada <code>More Information</code></li>
    <li>Click pada <code>View Certificate</code></li>
</ol>
<p>tambahkan subdomain yang baru kamu temukan di <code>/etc/hosts</code></p>
<p><code>curl --head {subdomain}.futurevera.thm</code> flag kamu akan berada di <code>Location:</code></p>
<h3>Kesimpulan</h3>
<ul>
    <li>Subdomain enumeration saja tidak selalu cukup</li>
    <li>SSL certificate dapat menjadi sumber informasi penting</li>
    <li>Kombinasi teknik sangat membantu dalam menemukan celah</li>
</ul>
<h3>Pembelajaran</h3>
<ul>
    <li>🔍 Jangan hanya fokus pada satu teknik</li>
    <li>🔐 Selalu cek SSL/TLS certificate</li>
    <li>🔁 Adaptasi sangat penting saat mengalami kebuntuan</li>
</ul>
<h3>Tools yang Digunakan</h3>
<ul>
    <li>ffuf</li>
    <li>curl</li>
    <li>Browser (untuk analisis certificate)</li>
</ul>
