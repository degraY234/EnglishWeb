# EngPortal TOEIC System - User Guide

## 🎯 Sistem TOEIC Yang Diperbaiki

Sistem TOEIC EngPortal telah diupgrade dengan fitur-fitur profesional untuk memastikan fair evaluation dan engagement maksimal.

---

## 📋 Requirements untuk Mengikuti Ujian

### 1. **Minimal 500 XP**
User HARUS mencapai 500 XP sebelum bisa ikut ujian TOEIC. Ini memastikan user sudah belajar cukup sebelum evaluasi.

**Cara mengumpulkan XP:**
- 📖 **Daily Challenge**: +20 XP per hari (2x XP multiplier)
- 🎯 **Regular Quiz**: +10 XP per soal
- 📚 **Vocabulary**: +5 XP per kata di flashcard

💡 **Pro Tip**: Kerjakan Daily Challenge setiap hari untuk akumulasi XP cepat

### 2. **30-Day Cooldown Lock**
Setelah mengambil ujian, user TIDAK bisa mengambil ujian lagi dalam 30 hari.

**Alasan:**
- Memastikan evaluasi yang fair
- Memberi waktu user untuk belajar lebih
- Mencegah "ujian spamming"

---

## 🎓 Exam Format Baru

### Struktur Ujian (12 Soal, 45 Menit)

#### 1. **📻 Listening Section (5 Soal)**
Soal berbentuk audio yang akan dibacakan browser (Web Speech API).

**Cara Mengerjakan:**
1. Baca pertanyaan di layar
2. Klik tombol **🔊 Play Audio** untuk dengarkan audio
3. Pilih jawaban yang tepat berdasarkan apa yang didengar
4. Lanjut ke soal berikutnya

**Contoh Soal:**
- "When is the meeting scheduled?"
- "What is the percentage increase?"
- "Where is the new office located?"

---

#### 2. **📖 Reading Section (4 Soal)**
Passage pendek diikuti dengan pertanyaan pemahaman.

**Cara Mengerjakan:**
1. Baca passage yang diberikan
2. Baca pertanyaan dengan teliti
3. Pilih jawaban yang paling sesuai
4. Lanjut ke soal berikutnya

**Contoh Soal:**
- Passage tentang: Company profile, Training program, Business updates, Financial reports
- Pertanyaan: Comprehension, Detail finding, Main idea

---

#### 3. **✍️ Grammar Section (3 Soal)**
Soal tata bahasa dengan incomplete sentences.

**Cara Mengerjakan:**
1. Baca kalimat dengan blank (___)
2. Pilih opsi grammar yang paling tepat
3. Pertimbangkan konteks dan tense

**Contoh Soal:**
- "She ___ to the meeting yesterday" → answer: went
- "This is ___ book I've read" → answer: the most interesting
- "If I ___ you, I would..." → answer: were

---

## 🏆 Scoring System

**Skala: 0-990 (TOEIC Standard)**

Scoring breakdown:
- **Listening**: 0-250
- **Reading**: 0-250
- **Grammar**: 0-250
- **Total**: 0-990

**Grade Reference:**
- 850+: 🌟 Outstanding
- 750+: 🏆 Excellent
- 600+: 👍 Good
- 450+: 📖 Fair
- Below 450: 💪 Keep Learning

---

## ✨ Fitur-Fitur Baru

### 1. **Web Speech API untuk Listening**
Browser membacakan audio soal secara otomatis dengan suara yang jelas.

```javascript
// Klik 🔊 Play Audio untuk mendengar pertanyaan
// Sistem akan memutar audio dengan:
// - Rate: 0.9 (slower untuk clarity)
// - Volume: 0.8 (comfortable level)
```

### 2. **Random Question Generator**
Setiap ujian menggunakan 12 soal yang berbeda dari database 25+ soal.

**Keuntungan:**
- ❌ Tidak akan ketemu soal yang sama setiap bulan
- ✅ Fair testing untuk semua user
- 🔄 Variasi soal yang luas

### 3. **Detailed Score Breakdown**
Hasil ujian menampilkan skor per section, bukan hanya total.

```
Result Summary:
┌─────────────┬────────────┐
│ Section     │ Score      │
├─────────────┼────────────┤
│ 📻 Listening│ 200/250    │
│ 📖 Reading  │ 180/250    │
│ ✍️ Grammar  │ 220/250    │
├─────────────┼────────────┤
│ 🏆 TOTAL    │ 600/990    │
└─────────────┴────────────┘
```

### 4. **Certificate of Achievement**
Jika skor ≥ 700, user mendapat certificate yang bisa diunduh.

---

## 🚀 Step-by-Step Guide

### First Time Attempt:

```
1. Login dengan username Anda
2. Klik "Daily Challenge" 
3. Complete Daily Challenge setiap hari
4. Kumpulkan XP sampai mencapai 500 XP
5. Buka menu "Monthly TOEIC"
6. Klik tombol "Start Exam" (akan enabled setelah 500 XP)
7. Selesaikan 12 soal dalam 45 menit
8. Lihat hasil breakdown + certificate (jika applicable)
```

### Attempt Berikutnya:

```
1. Tunggu 30 hari dari exam terakhir
2. UI akan menunjukkan countdown: "Available in X days"
3. Kembali ke menu TOEIC setelah 30 hari
4. Button akan enabled, klik untuk mulai
5. Sistem akan generate 12 soal berbeda
```

---

## 🔒 Security & Fair Play

### Proteksi Sistem:

1. **LocalStorage Validation**
   - Semua data disimpan aman di localStorage
   - Automatic validation on load
   - Prevent data tampering

2. **Console Bypass Detection**
   - Jika user coba bypass lewat console: warning ditampilkan
   - System logs attempt: "Unauthorized access attempt detected"

3. **Strict 30-Day Lock**
   - Bahkan jika user clear localStorage, system tetap track
   - Jika data missing, reset ke default

### Fair Play Indicators:

✅ Soal acak setiap ujian
✅ Waktu terbatas (45 menit)
✅ Cooling period antar ujian (30 hari)
✅ XP requirement untuk akses
✅ Audit trail lengkap

---

## 📊 Progress Tracking

### View Your History:
- Menu **Profile** → Lihat semua exam history
- Chart visual menampilkan trend score
- Best score dan average score

### Export Data:
Semua data tersimpan di browser localStorage:
```javascript
localStorage.getItem('engportal_user')
// Berisi: XP, exam history, flashcards, dll
```

---

## ❓ FAQ

**Q: Berapa kali saya bisa ikut ujian?**
A: 1 kali per 30 hari setelah mencapai 500 XP.

**Q: Bagaimana cara cepat mengumpulkan XP?**
A: Lakukan Daily Challenge setiap hari (20 XP/hari) = 500 XP dalam ~25 hari.

**Q: Bisakah saya reset XP untuk ujian baru?**
A: Tidak, sistem dirancang untuk fair play. Logout dan login dengan akun baru untuk fresh start.

**Q: Apakah jawaban akan tersimpan jika time habis?**
A: Ya, sistem akan auto-submit ketika waktu habis.

**Q: Bagaimana jika audio tidak jalan?**
A: Periksa browser support, setting volume, dan speaker. Chrome/Edge/Safari support Web Speech API.

---

## 🎯 Tips for Better Performance

1. **Prepare First**
   - Complete Daily Challenge for 25 days = 500 XP
   - Practice Regular Quiz to familiarize
   - Read flashcards untuk vocabulary

2. **During Exam**
   - Manage waktu: 45 menit / 12 soal = 3.75 min/soal
   - Listening soal: Dengarkan dengan teliti
   - Reading soal: Highlight keyword dalam passage
   - Grammar soal: Think about tense dan agreement

3. **After Exam**
   - Review score breakdown
   - Identify weak sections
   - Focus on weak areas untuk improvement

---

## 📞 Support

Jika ada pertanyaan atau issue:
1. Check browser console (F12) untuk error logs
2. Verify localStorage data: `localStorage.getItem('engportal_user')`
3. Try clearing browser cache dan reload
4. Ensure JavaScript enabled dan browser updated

---

**Last Updated**: May 2026
**System Version**: 2.0 (Improved TOEIC)
**Status**: ✅ Production Ready

