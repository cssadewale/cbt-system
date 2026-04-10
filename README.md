# CBT Pro — Free, CSV-Powered Computer Based Testing System

> *Built by a tutor who got tired of typing exam questions one by one.*

![Status](https://img.shields.io/badge/Status-Live-10b981?style=flat-square)
![Stack](https://img.shields.io/badge/Stack-HTML%20%2F%20CSS%20%2F%20Vanilla%20JS-blue?style=flat-square)
![Hosting](https://img.shields.io/badge/Hosting-GitHub%20Pages-black?style=flat-square)
![Database](https://img.shields.io/badge/Database-Supabase-3ECF8E?style=flat-square)
![Cost](https://img.shields.io/badge/Total%20Cost-₦0-10b981?style=flat-square)
![Device](https://img.shields.io/badge/Built%20On-Android%20Tablet-f59e0b?style=flat-square)

---

## 🔗 Live Demo

| Portal | Link |
|--------|------|
| 👨‍🏫 Teacher / Admin Dashboard | [cssadewale.github.io/cbt-system/teacher.html](https://cssadewale.github.io/cbt-system/teacher.html) |
| 🎓 Student Exam Portal | [cssadewale.github.io/cbt-system/student.html](https://cssadewale.github.io/cbt-system/student.html) |

---

## 🎯 The Problem This Solves

I have been a Mathematics, Further Mathematics, Physics and Chemistry tutor for over 10 years. I set CBT exams regularly for my students — for WAEC practice, JAMB drills, terminal assessments, and class tests.

Every CBT platform I found had the same frustrating pattern:
- Type questions **one by one** into a form — slow, error-prone, exhausting
- CSV upload only available on **expensive paid plans**
- Free plans with **severe feature limitations**

Meanwhile, I was already generating exam questions with AI and exporting them to CSV in seconds. The gap between what I had and what the platforms offered made no sense.

So I built my own. Free. Open source. CSV-first. With every feature I actually needed — and a few the paid platforms don't even have.

---

## ✨ Features

### 👨‍🏫 For Teachers / Administrators
| Feature | Description |
|---------|-------------|
| 🔐 Secure auth | Register and sign in with email and password |
| 📄 CSV upload | Upload one file — full exam ready in seconds |
| ⚙️ Exam configuration | Set subject, duration, question count, and attempt limit per exam |
| 🔀 Question randomisation | Questions pulled randomly from your question bank per student |
| 👁️ Exam preview | Preview all questions and correct answers before publishing |
| 🔗 Instant sharing | Auto-generated shareable exam link — send directly via WhatsApp |
| 🔒 Exam control | Lock or unlock exams at any time |
| 📊 Results dashboard | View all student submissions with scores, time taken, and attempt number |
| 🔍 Search & filter | Search results by student name or class |
| ⬇️ CSV export | Download all results as a spreadsheet |

### 🎓 For Students
| Feature | Description |
|---------|-------------|
| 🔗 Direct link access | No account needed — click the link and start |
| ⏱️ Countdown timer | Live timer with minutes and seconds |
| 🧭 Question navigator | Jump to any question, see answered/unanswered at a glance |
| 🔀 Randomised options | Answer options shuffled per student — makes copying impossible |
| 🧮 Scientific calculator | Built-in calculator with sin, cos, tan, log, √, x² — ideal for Maths and Physics |
| 📝 Attempt limit enforcement | System checks attempt history before allowing entry |
| 🛡️ Anti-cheat measures | Tab-switching detection — exam auto-submits after 2 violations |
| 📋 Instant results | Score and full answer review immediately after submission |
| 💡 Explanations | Teacher-written explanations shown for wrong answers |

---

## 🗂️ Project Structure

```
cbt-system/
│
├── teacher.html      # Admin portal — authentication, exam creation, results
├── student.html      # Student portal — exam engine, calculator, results
│
├── sample/
│   └── further_maths_sample.csv   # Sample CSV question file
│
└── README.md
```

---

## 📋 CSV Question Format

Each exam is powered by a single CSV file. This is the format:

```csv
"Question","A","B","C","D","Answer","Explanation"
"Find lim(x→2) (x²-4)/(x-2)","Undefined","4","2","0","B","Factor as (x-2)(x+2). Cancel (x-2) to get x+2. At x=2 this equals 4."
"Differentiate y = x³eˣ","3x²eˣ + x³eˣ","3x²eˣ","3x²eˣ - x³eˣ","x³eˣ","A","Product rule: (x³)'eˣ + x³(eˣ)' = 3x²eˣ + x³eˣ."
```

| Column | Field | Required |
|--------|-------|----------|
| 1 | Question text | ✅ |
| 2 | Option A | ✅ |
| 3 | Option B | ✅ |
| 4 | Option C | ✅ |
| 5 | Option D | ✅ |
| 6 | Correct answer (A / B / C / D) | ✅ |
| 7 | Explanation for wrong answers | ⬜ Optional |

**Notes:**
- Wrap all cells in double quotes
- The Explanation column is optional — existing CSVs without it still work
- You can generate questions with AI and export directly to this format
- Yoruba-language CSVs should be saved with UTF-8-BOM encoding

---

## 🗄️ Database Schema (Supabase)

```sql
-- Exams table
create table exams (
  id uuid default gen_random_uuid() primary key,
  teacher_id uuid references auth.users(id) on delete cascade,
  subject text not null,
  duration integer not null,
  attempt_limit integer default 1,
  select_count integer default 0,
  code text unique not null,
  csv_data jsonb not null,
  is_open boolean default true,
  created_at timestamp with time zone default now()
);

-- Results table
create table results (
  id uuid default gen_random_uuid() primary key,
  exam_id uuid references exams(id) on delete cascade,
  student_name text not null,
  student_class text not null,
  score integer not null,
  total integer not null,
  attempt_number integer default 1,
  time_taken integer,
  created_at timestamp with time zone default now()
);

-- Row Level Security
alter table exams enable row level security;
alter table results enable row level security;

-- Teachers manage their own exams
create policy "teacher_manage_exams"
  on exams for all
  using (auth.uid() = teacher_id)
  with check (auth.uid() = teacher_id);

-- Public can read exams by code (students need this)
create policy "public_read_exams"
  on exams for select using (true);

-- Anyone can insert results (students submitting)
create policy "public_insert_results"
  on results for insert with check (true);

-- Anyone can read results (for attempt checking)
create policy "public_select_results"
  on results for select using (true);
```

---

## 🚀 Deploy Your Own Instance

This platform is designed for **zero-cost self-hosting**. You need a GitHub account and a free Supabase account. Nothing else.

### Step 1 — Set Up Supabase

1. Create a free project at [supabase.com](https://supabase.com)
2. Go to **SQL Editor → New Query** and run the schema above
3. Go to **Authentication → Providers → Email** → Enable email sign-in → Disable "Confirm email" for development
4. Go to **Settings → API** and copy your:
   - **Project URL** (e.g. `https://xxxxxx.supabase.co`)
   - **anon public key** (starts with `eyJ...`)

### Step 2 — Configure the Files

In both `teacher.html` and `student.html`, find these two lines near the top of the `<script>` section and replace with your values:

```javascript
const SB_URL = 'https://your-project-id.supabase.co';
const SB_KEY = 'your-anon-public-key';
```

### Step 3 — Deploy to GitHub Pages

1. Fork this repository
2. Go to **Settings → Pages → Source: Deploy from branch → Branch: main → Save**
3. Your live URLs will be:
   ```
   https://yourusername.github.io/cbt-system/teacher.html
   https://yourusername.github.io/cbt-system/student.html
   ```

### Step 4 — Share With Students

Create an exam on the teacher dashboard, copy the generated link, and send it to students via WhatsApp. They click, enter their details, and the exam begins — no account, no app download, no confusion.

---

## 🔒 Security Notes

- The Supabase **anon key** is designed for browser use — it is safe in frontend code
- Row Level Security ensures teachers can only access their own exams and results
- **Never** put your Supabase `service_role` key in any frontend file
- Students do not need accounts — they access exams by unique link only
- Tab-switching detection auto-submits the exam after 2 violations

---

## 🛠️ Tech Stack

| Layer | Technology | Cost |
|-------|-----------|------|
| Frontend | HTML5, CSS3, Vanilla JavaScript | Free |
| Hosting | GitHub Pages | Free |
| Database | Supabase (PostgreSQL) | Free tier |
| Authentication | Supabase Auth | Free tier |
| Fonts | Google Fonts (Plus Jakarta Sans) | Free |
| Editor | Acode (Android) | Free |

**No frameworks. No npm. No build tools. No Node.js. Just two HTML files that work.**

---

## 📱 Built Entirely on an Android Tablet

Every line of code in this project was written, tested, and deployed from an **Android tablet** using the **Acode editor**. No laptop. No PC. No desktop IDE.

This was a deliberate constraint — not a limitation. It proves that the barrier to building real, production-grade software is not hardware. It is **clarity of thought and willingness to learn**.

---

## 🧠 About the Builder

**Adewale Samson Adeagbo**

I am a secondary school tutor with over 10 years of experience teaching Mathematics, Further Mathematics, Physics, and Chemistry at JSS and SSS level — both virtually and in person across Lagos and Ogun State, Nigeria.

I began my data science journey in 2024. I can write and read Python and SQL, handle end-to-end data science projects, work with GitHub, and deploy models on Streamlit. I am comfortable at an intermediate level and growing.

This project was born from a simple frustration: I could not find a free CBT platform that let me upload a CSV. Every tool I found made me type questions one by one, or charged money for the feature I needed. A colleague showed me that AI could guide someone with limited web experience to build real software — and that idea would not leave me alone.

So I built this. From scratch. On a tablet. With AI as my guide and a real problem as my north star.

> *"Data science taught me that if you can think through a problem clearly, you can find a path to a solution — even in unfamiliar territory."*

---

## 🤝 Connect

- 📧 [buildingmyictcareer@gmail.com](mailto:buildingmyictcareer@gmail.com)
- 💼 [linkedin.com/in/adewalesamsonadeagbo](https://linkedin.com/in/adewalesamsonadeagbo)
- 🐙 [github.com/cssadewale](https://github.com/cssadewale)

---

## 🤝 Contributing

This project is open. If you are a Nigerian educator, developer, or anyone who sees value in free EdTech tools — contributions are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-idea`
3. Commit your changes
4. Open a Pull Request with a clear description of what changed and why

**Before contributing, please read the project values:**
- Free first — no feature should require a paid service
- Mobile-friendly — many Nigerian students use phones, not laptops
- Simple — the code should be readable by someone learning, not just experts
- Backward compatible — new CSV columns must remain optional

---

## 📄 License

MIT License — free to use, modify, and distribute. Attribution appreciated but not required.

---

<div align="center">
  <strong>CBT Pro</strong> · Built with frustration, curiosity, and an Android tablet.<br/>
  <sub>If this saved you time, give it a ⭐ on GitHub. If it helped a student, that's enough.</sub>
</div>
