<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=30&pause=1000&color=25D366&center=true&vCenter=true&width=600&lines=🤖+Ashi+Bot+WhatsApp+API;Built+on+Socketon+%2B+Baileys;By+Wily+Kun+%7C+2026" alt="Typing SVG" />

<br/>

<img src="https://img.shields.io/badge/Version-1.8.28-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" />
<img src="https://img.shields.io/badge/Node.js-%3E%3D20.0.0-339933?style=for-the-badge&logo=node.js&logoColor=white" />
<img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" />
<img src="https://img.shields.io/badge/Status-Active-success?style=for-the-badge&logo=statuspage&logoColor=white" />

<br/><br/>

> **Socketon** adalah modifikasi WhatsApp API berbasis **Baileys** yang dikembangkan untuk kebutuhan bot WhatsApp berfitur lengkap, stabil, dan aman.  
> Digunakan dan dikembangkan oleh **𝗪𝗶𝗹𝘆 𝗞𝘂𝗻** untuk **𝗔𝘀𝗵𝗶 𝗕𝗼𝘁**.

</div>

---

## 👑 Owner / Pengembang

<div align="center">

| Info | Detail |
|------|--------|
| 👤 **Nama** | 𝗪𝗶𝗹𝘆 𝗞𝘂𝗻 |
| 🤖 **Nama Bot** | 𝗔𝘀𝗵𝗶 𝗕𝗼𝘁 |
| 📱 **WhatsApp** | [089-688-206-739](https://wa.me/6289688206739) |
| 📢 **Telegram** | [@Wilykun1994](https://t.me/Wilykun1994) |

</div>

### 🌐 Media Sosial

<div align="center">

[![WhatsApp](https://img.shields.io/badge/WhatsApp-Chat-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/6289688206739)
[![Telegram](https://img.shields.io/badge/Telegram-Chat-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/Wilykun1994)
[![Facebook](https://img.shields.io/badge/Facebook-Profile-1877F2?style=for-the-badge&logo=facebook&logoColor=white)](https://www.facebook.com/wily.kun.1)
[![YouTube](https://img.shields.io/badge/YouTube-Channel-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://youtube.com/@ukosamasomoni1956)
[![Email](https://img.shields.io/badge/Email-Kirim%20Pesan-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:kunwily1994@gmail.com)

</div>

### 🔗 Grup & Channel Ashi Bot

<div align="center">

[![GC Bot V1](https://img.shields.io/badge/Grup%20WhatsApp-V1-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://chat.whatsapp.com/KOHWg6v5GCc9pxpVIj0GtB)
[![GC Bot V2](https://img.shields.io/badge/Grup%20WhatsApp-V2-128C7E?style=for-the-badge&logo=whatsapp&logoColor=white)](https://chat.whatsapp.com/H9UeMjqecE94rKo4NnVAP8)
[![Channel V1](https://img.shields.io/badge/Channel%20WA-V1-075E54?style=for-the-badge&logo=whatsapp&logoColor=white)](https://whatsapp.com/channel/0029VaiyhS37IUYSuDJoJj1L)
[![Channel V2](https://img.shields.io/badge/Channel%20WA-V2-075E54?style=for-the-badge&logo=whatsapp&logoColor=white)](https://whatsapp.com/channel/0029Vavn0K2IHphFVpTvgz1V)

</div>

---

## 📦 Instalasi

```bash
npm install socketon
```

**Persyaratan:** Node.js >= 20.0.0

---

## ⚡ Quick Start

```javascript
const { makeWASocket, useMultiFileAuthState, DisconnectReason } = require('socketon');
const pino = require('pino');

async function start() {
    const { state, saveCreds } = await useMultiFileAuthState('./auth');
    
    const sock = makeWASocket({
        logger: pino({ level: 'silent' }),
        auth: state,
        printQRInTerminal: true
    });

    sock.ev.on('connection.update', async (update) => {
        const { connection, lastDisconnect } = update;
        if (connection === 'close') {
            const shouldReconnect = lastDisconnect?.error?.output?.statusCode !== DisconnectReason.loggedOut;
            if (shouldReconnect) start();
        } else if (connection === 'open') {
            console.log('✅ Ashi Bot Terhubung!');
        }
    });

    sock.ev.on('messages.upsert', async ({ messages }) => {
        const msg = messages[0];
        if (!msg.key.fromMe && msg.message) {
            console.log('📩 Pesan baru:', msg);
        }
    });

    sock.ev.on('creds.update', saveCreds);
}

start();
```

---

## 🚀 Fitur Lengkap

<details>
<summary><b>💬 Pesan (Messages)</b></summary>

### Kirim Berbagai Jenis Pesan

```javascript
// Teks biasa
await sock.sendMessage(jid, { text: 'Halo dari Ashi Bot! 👋' });

// Gambar
await sock.sendMessage(jid, { image: { url: './foto.jpg' }, caption: 'Caption' });

// Video
await sock.sendMessage(jid, { video: { url: './video.mp4' }, caption: 'Video' });

// Audio
await sock.sendMessage(jid, { audio: { url: './audio.mp3' }, mimetype: 'audio/mp4', ptt: true });

// Dokumen
await sock.sendMessage(jid, { document: { url: './file.pdf' }, mimetype: 'application/pdf', fileName: 'file.pdf' });

// Stiker
await sock.sendMessage(jid, { sticker: { url: './sticker.webp' } });

// Lokasi
await sock.sendMessage(jid, { location: { degreesLatitude: -6.2, degreesLongitude: 106.8 } });

// Kontak
await sock.sendMessage(jid, { contacts: { displayName: 'Wily Kun', contacts: [{ vcard: '...' }] } });

// Reaksi pesan
await sock.sendMessage(jid, { react: { key: msg.key, text: '🔥' } });

// Balas pesan (quoted)
await sock.sendMessage(jid, { text: 'Balasan' }, { quoted: msg });

// Hapus pesan
await sock.sendMessage(jid, { delete: msg.key });

// Edit pesan (baru ✨)
await sock.sendMessage(jid, { edit: msg.key, text: 'Teks yang sudah diedit' });
```

</details>

<details>
<summary><b>📊 Poll / Jajak Pendapat (BARU ✨)</b></summary>

Kirim poll interaktif langsung dari bot!

```javascript
// Kirim poll
await sock.sendPollMessage(
    jid,
    'Pilih menu favorit kamu:',       // judul poll
    ['Nasi Goreng', 'Mie Ayam', 'Soto'], // pilihan
    1                                  // jumlah pilihan yang bisa dipilih
);

// Poll dengan multi-pilih
await sock.sendPollMessage(
    jid,
    'Pilih hobi kamu (boleh lebih dari satu):',
    ['Gaming', 'Coding', 'Musik', 'Olahraga'],
    3
);
```

</details>

<details>
<summary><b>✏️ Edit & Hapus Pesan (BARU ✨)</b></summary>

```javascript
// Edit pesan yang sudah dikirim
const sentMsg = await sock.sendMessage(jid, { text: 'Pesan awal' });
await sock.editMessage(jid, sentMsg, { text: 'Pesan sudah diedit!' });

// Hapus pesan (untuk semua orang)
await sock.sendMessage(jid, { delete: sentMsg.key });
```

</details>

<details>
<summary><b>👥 Grup</b></summary>

```javascript
// Buat grup baru
await sock.groupCreate('Nama Grup', ['628xxx@s.whatsapp.net']);

// Tambah/hapus/promote/demote anggota
await sock.groupParticipantsUpdate(jid, ['628xxx@s.whatsapp.net'], 'add');    // tambah
await sock.groupParticipantsUpdate(jid, ['628xxx@s.whatsapp.net'], 'remove'); // hapus
await sock.groupParticipantsUpdate(jid, ['628xxx@s.whatsapp.net'], 'promote'); // jadikan admin
await sock.groupParticipantsUpdate(jid, ['628xxx@s.whatsapp.net'], 'demote');  // turunkan admin

// Info grup
await sock.groupMetadata(jid);

// Kode undangan
await sock.groupInviteCode(jid);

// Perbarui nama & deskripsi grup
await sock.groupUpdateSubject(jid, 'Nama Grup Baru');
await sock.groupUpdateDescription(jid, 'Deskripsi baru');

// Pesan hilang (ephemeral)
await sock.groupToggleEphemeral(jid, 604800); // 7 hari

// Terima undangan grup
await sock.groupAcceptInvite('kode_undangan');

// Tinggalkan grup
await sock.groupLeave(jid);

// Mode tambah anggota (admin only / semua anggota)
await sock.groupMemberAddMode(jid, 'admin_add'); // atau 'all_member_add'

// Kelola permintaan bergabung
await sock.groupRequestParticipantsList(jid);
await sock.groupRequestParticipantsUpdate(jid, ['628xxx@s.whatsapp.net'], 'approve'); // atau 'reject'
```

</details>

<details>
<summary><b>📰 Newsletter / Channel WhatsApp</b></summary>

```javascript
// Buat newsletter
await sock.newsletterCreate('Nama Channel', 'Deskripsi', ['👍']);

// Ikuti/berhenti ikuti newsletter
await sock.newsletterFollow(newsletterJid);
await sock.newsletterUnfollow(newsletterJid);

// Info newsletter
await sock.newsletterMetadata('invite', 'kode');

// Kirim reaksi ke pesan newsletter
await sock.newsletterReactMessage(newsletterJid, serverId, '🔥');
```

</details>

<details>
<summary><b>🔐 Autentikasi</b></summary>

```javascript
// QR Code (default)
const sock = makeWASocket({ printQRInTerminal: true });

// Pairing Code (tanpa scan QR)
const sock = makeWASocket({ printQRInTerminal: false });
const code = await sock.requestPairingCode('628xxxxxxxxxx');
console.log('Kode pairing:', code);

// Simpan sesi
const { state, saveCreds } = await useMultiFileAuthState('./auth_folder');
sock.ev.on('creds.update', saveCreds);
```

</details>

<details>
<summary><b>👤 Profil & Privasi</b></summary>

```javascript
// Update foto profil
await sock.updateProfilePicture(jid, { url: './foto.jpg' });

// Update nama profil
await sock.updateProfileName('Ashi Bot 🤖');

// Update status/bio
await sock.updateProfileStatus('Hai! Aku Ashi Bot 🤖');

// Ambil foto profil
const url = await sock.profilePictureUrl(jid, 'image');

// Cek status/bio kontak
const status = await sock.fetchStatus(jid);

// Cek privasi
const privacy = await sock.fetchPrivacySettings();

// Blokir kontak
await sock.updateBlockStatus(jid, 'block');

// Buka blokir kontak
await sock.updateBlockStatus(jid, 'unblock');

// Daftar kontak yang diblokir
const blocklist = await sock.fetchBlocklist();
```

</details>

<details>
<summary><b>💡 Presence (Indikator Mengetik / Merekam)</b></summary>

```javascript
// Tampilkan indikator "sedang mengetik..."
await sock.sendPresenceUpdate('composing', jid);

// Tampilkan indikator "sedang merekam audio..."
await sock.sendPresenceUpdate('recording', jid);

// Hentikan indikator
await sock.sendPresenceUpdate('paused', jid);

// Subscribe ke presence kontak lain
await sock.presenceSubscribe(jid);
```

</details>

<details>
<summary><b>⭐ Bintang & Modifikasi Chat</b></summary>

```javascript
// Bintangi pesan
await sock.star(jid, [{ id: msg.key.id, fromMe: false }], true);

// Hapus bintang pesan
await sock.star(jid, [{ id: msg.key.id, fromMe: false }], false);

// Modifikasi chat (arsip, pin, mute, dll)
await sock.chatModify({ archive: true }, jid);      // arsipkan
await sock.chatModify({ pin: true }, jid);           // pin chat
await sock.chatModify({ mute: 8 * 60 * 60 * 1000 }, jid); // mute 8 jam
await sock.chatModify({ markRead: false }, jid);     // tandai belum dibaca
```

</details>

<details>
<summary><b>🏪 Bisnis & Katalog</b></summary>

```javascript
// Ambil katalog produk
await sock.getCatalog({ jid });

// Buat produk
await sock.productCreate({ name: 'Produk', price: 10000, currency: 'IDR' });

// Update produk
await sock.productUpdate(productId, { price: 15000 });

// Hapus produk
await sock.productDelete([productId]);

// Detail pesanan
await sock.getOrderDetails(orderId, tokenBase64);
```

</details>

---

## 📡 Events

```javascript
sock.ev.on('connection.update', callback)   // Status koneksi
sock.ev.on('messages.upsert', callback)     // Pesan masuk baru
sock.ev.on('messages.update', callback)     // Update pesan (baca, dll)
sock.ev.on('presence.update', callback)     // Update presence (mengetik, dll)
sock.ev.on('groups.upsert', callback)       // Grup baru
sock.ev.on('groups.update', callback)       // Update info grup
sock.ev.on('group-participants.update', callback) // Update anggota grup
sock.ev.on('chats.upsert', callback)        // Chat baru
sock.ev.on('contacts.upsert', callback)     // Kontak baru
sock.ev.on('creds.update', callback)        // Kredensial diperbarui
```

---

## 🆕 Perubahan & Pembaruan

### v1.8.28 — Terbaru ✅
- ✨ **[BARU]** `sendPollMessage()` — Kirim poll/jajak pendapat interaktif
- ✨ **[BARU]** `editMessage()` — Edit pesan yang sudah terkirim
- ✅ `groupRequestParticipantsList` & `groupRequestParticipantsUpdate` — Kelola permintaan bergabung grup
- ✅ `groupMemberAddMode` & `groupMembershipApprovalMode` — Mode bergabung grup
- ✅ `sendPresenceUpdate` — Indikator mengetik/merekam
- ✅ `star` — Bintangi/hapus bintang pesan
- ✅ `fetchStatus`, `fetchBlocklist`, `updateBlockStatus` — Fitur profil & privasi lengkap
- ✅ Support penuh Newsletter / Channel WhatsApp
- ✅ Sticker pack support (`𝗔𝘀𝗵𝗶 𝗕𝗼𝘁` | `By ©𝗪𝗶𝗹𝘆`)

---

## ⚙️ Konfigurasi Stiker (WM Sticker)

```javascript
global.stickpack = '𝗔𝘀𝗵𝗶 𝗕𝗼𝘁';
global.stickauth = 'By ©𝗪𝗶𝗹𝘆';
```

---

## 📋 Syarat Penggunaan

1. **Atribusi** — Tetap cantumkan kredit kepada pengembang.
2. **Tidak Boleh Disalahgunakan** — Dilarang untuk spam, penipuan, atau aktivitas berbahaya.
3. **Patuhi WhatsApp ToS** — Pengguna bertanggung jawab atas kepatuhan terhadap Ketentuan Layanan WhatsApp.
4. **Tanpa Garansi** — Library ini disediakan "apa adanya".
5. **Kepatuhan Lisensi MIT** — Wajib mematuhi ketentuan MIT License.

---

## 🤝 Dukungan

<div align="center">

Butuh bantuan atau punya pertanyaan? Hubungi langsung:

[![WhatsApp](https://img.shields.io/badge/Chat%20via%20WhatsApp-6289688206739-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/6289688206739)
[![Telegram](https://img.shields.io/badge/Chat%20via%20Telegram-@Wilykun1994-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)](https://t.me/Wilykun1994)

</div>

---

## 📄 Lisensi

**MIT License** — © 2024–2026 Socketon · Digunakan & dikembangkan oleh **𝗪𝗶𝗹𝘆 𝗞𝘂𝗻**

---

<div align="center">

**Made with ❤️ by [𝗪𝗶𝗹𝘆 𝗞𝘂𝗻](https://wa.me/6289688206739) for 𝗔𝘀𝗵𝗶 𝗕𝗼𝘁**

<img src="https://komarev.com/ghpvc/?username=ashibot-wilykun&label=Profile+Views&color=25D366&style=flat" alt="views" />

</div>
