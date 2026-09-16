# 1. Konsep Dasar Kecerdasan Buatan (Artificial Intelligence)

Navigasi: [[Konsep_Kecerdasan_Buatan]] | Modul Berikutnya: [[02_Intelligent_Agent_dan_Problem_Solving]]

---

**Kecerdasan Buatan (Artificial Intelligence / AI)** adalah cabang ilmu komputer yang berfokus pada pembuatan sistem komputer yang mampu melakukan tugas-tugas yang membutuhkan kecerdasan manusia, seperti pemecahan masalah, persepsi visual, pengenalan suara, dan pengambilan keputusan.

---

## 1.1. Empat Sudut Pandang Definisi AI

Secara historis, definisi Kecerdasan Buatan dikelompokkan ke dalam 4 kuadran pendekatan:

```mermaid
graph TD
    subgraph Human ["Dimensi Manusia"]
        TH["1. Thinking Humanly (Berpikir Seperti Manusia)"]
        AH["2. Acting Humanly (Bertindak Seperti Manusia)"]
    end

    subgraph Rational ["Dimensi Rasional"]
        TR["3. Thinking Rationally (Berpikir Rasional / Hukum Logika)"]
        AR["4. Acting Rationally (Bertindak Rasional / Agen Cerdas)"]
    end
```

| Pendekatan | Karakteristik Utama | Ujian / Standar Pengukuran |
| :--- | :--- | :--- |
| **Acting Humanly** | Komputer bertindak seperti manusia hingga tidak dapat dibedakan. | **Uji Turing (Turing Test)** ciptaan Alan Turing (1950). |
| **Thinking Humanly** | Memodelkan proses kerja otak dan kognitif manusia. | Ilmu Kognitif & Pemodelan Jaringan Saraf. |
| **Thinking Rationally**| Menggunakan kaidah hukum logika matematika (*Silogisme*). | Logika Proposisi & First-Order Logic (FOL). |
| **Acting Rationally** | Agen bertindak untuk mencapai hasil terbaik (*Rational Agent*). | **Agen Berbasis Tujuan & Utilitas (Standard Modern AI).** |

---

## 1.2. Uji Turing (Turing Test)

**Uji Turing** diajukan oleh Alan Turing pada tahun 1950 untuk menjawab pertanyaan *"Apakah mesin dapat berpikir?"*. 

```mermaid
sequenceDiagram
    participant Interrogator as Penguji Manusia (Interrogator)
    participant Wall as Dinding Pembatas
    participant Human as Manusia Asli (Subject A)
    participant AI as Komputer / AI (Subject B)

    Interrogator->>Wall: Kirim Pertanyaan Teks
    Wall->>Human: Teruskan ke Manusia
    Wall->>AI: Teruskan ke Komputer
    Human-->>Interrogator: Jawaban Teks
    AI-->>Interrogator: Jawaban Teks
    Note over Interrogator: Jika Penguji TIDAK BISA membedakan<br/>mana respon manusia dan mana respon AI,<br/>maka AI LULUS Turing Test!
```

---

Navigasi: [[Konsep_Kecerdasan_Buatan]] | Modul Berikutnya: [[02_Intelligent_Agent_dan_Problem_Solving]]
