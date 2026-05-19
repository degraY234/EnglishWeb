# EngPortal TOEIC System v2.0 - Architecture & Flow

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     EngPortal v2.0                              │
│                  TOEIC Assessment System                         │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  UI LAYER (HTML/CSS)                                            │
├─────────────────────────────────────────────────────────────────┤
│  • Dictionary Section       • Daily Challenge                    │
│  • Quiz Section            • TOEIC Exam (MAIN)                  │
│  • Flashcard Section       • Leaderboard                         │
│  • Profile Section         • Mobile Navigation                   │
└─────────────────────────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────┐
│  BUSINESS LOGIC LAYER (JavaScript Functions)                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────── EXAM CONTROL ──────────────────┐             │
│  │ • checkExamAvailability()  - 30day + XP check│             │
│  │ • startExam()              - Initialize       │             │
│  │ • displayExamQuestion()    - Show Q w/ audio  │             │
│  │ • selectExamAnswer()       - Save answer      │             │
│  │ • endExam()                - Timer or finish  │             │
│  │ • displayExamResults()     - Show breakdown   │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
│  ┌─────────────── SCORE CALCULATION ─────────────┐             │
│  │ • calculateExamScore()     - Per-section      │             │
│  │ • saveExamResult()         - Persist data     │             │
│  │ • renderTOEICChart()       - Visualization    │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
│  ┌─────────────── FEATURES ──────────────────────┐             │
│  │ • generateRandomExam()     - Random 12 Q     │             │
│  │ • speakQuestion()          - Web Speech API   │             │
│  │ • protectExamFromBypass()  - Security        │             │
│  │ • jumpToQuestion()         - Navigation      │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
│  ┌─────────────── UTILITIES ─────────────────────┐             │
│  │ • addXP()                  - XP System        │             │
│  │ • updateUI()               - Refresh display  │             │
│  │ • showNotification()       - User feedback    │             │
│  │ • loadUser() / saveUser()  - Data persist     │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────────┐
│  DATA LAYER                                                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────── DATABASE ──────────────────────┐             │
│  │ toeicDatabase {                               │             │
│  │   listening: [5 soal],                        │             │
│  │   reading: [5 soal],                          │             │
│  │   grammar: [10 soal]                          │             │
│  │ }                                             │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
│  ┌─────────────── USER STATE ────────────────────┐             │
│  │ {                                             │             │
│  │   name, level, xp, maxXp, streak,            │             │
│  │   flashcards, history,                        │             │
│  │   lastExamDate (ISO),                         │             │
│  │   toeicHistory: [{                            │             │
│  │     date, score, listening, reading,          │             │
│  │     grammar, timestamp                        │             │
│  │   }],                                         │             │
│  │   bestTOEIC, totalExamsTaken                  │             │
│  │ }                                             │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
│  ┌─────────────── STORAGE ───────────────────────┐             │
│  │ localStorage.setItem('engportal_user', JSON) │             │
│  └────────────────────────────────────────────────┘             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│  EXTERNAL APIs                                                  │
├─────────────────────────────────────────────────────────────────┤
│  • window.speechSynthesis (Web Speech API)                     │
│  • window.localStorage (Browser Storage API)                   │
│  • window.AudioContext (Audio Synthesis)                       │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Exam Taking Flow (Detailed)

```
START EXAM FLOW
│
├─ USER CLICKS "START EXAM"
│  └─ startExam() called
│
├─ CHECK AVAILABILITY
│  ├─ Check: XP >= 500?
│  │  ├─ NO → showNotification() → STOP
│  │  └─ YES → continue
│  │
│  └─ Check: 30 day cooldown?
│     ├─ lastExamDate not set → FIRST TIME
│     │  └─ Continue (can start)
│     │
│     ├─ lastExamDate + 30 days < today → ELIGIBLE
│     │  └─ Continue (can start)
│     │
│     └─ lastExamDate + 30 days > today → LOCKED
│        └─ showNotification(countdown) → STOP
│
├─ INITIALIZE EXAM
│  ├─ generateRandomExam() 
│  │  ├─ Shuffle all 25+ soal
│  │  ├─ Pick random 12
│  │  └─ Balance: Listening, Reading, Grammar
│  │
│  ├─ Set currentExam = [12 soal]
│  ├─ Reset examAnswers = {}
│  ├─ Set timeLeft = 45 * 60 seconds
│  └─ Set examActive = true
│
├─ START UI & TIMER
│  ├─ Hide examWelcome
│  ├─ Show examActive
│  ├─ Start timer → displayExamQuestion(0)
│  └─ renderQuestionNav()
│
├─ DISPLAY QUESTION 1
│  ├─ Get Q from currentExam[0]
│  │
│  ├─ IF type = "listening"
│  │  ├─ Show audioText
│  │  ├─ Show "🔊 Play Audio" button
│  │  ├─ When clicked → speakQuestion(audioText)
│  │  │  └─ Browser speak via speechSynthesis
│  │  └─ Show question & options
│  │
│  ├─ IF type = "reading"
│  │  ├─ Show passage
│  │  ├─ Show question below
│  │  └─ Show 4 options
│  │
│  └─ IF type = "grammar"
│     ├─ Show incomplete sentence
│     └─ Show 4 grammar options
│
├─ USER INTERACTION LOOP
│  │
│  ├─ USER SELECTS ANSWER
│  │  ├─ selectExamAnswer(qIndex, optionIndex)
│  │  ├─ Save: examAnswers[qIndex] = optionIndex
│  │  ├─ Update nav dot → blue (answered)
│  │  └─ Highlight selected option
│  │
│  ├─ USER NAVIGATES
│  │  ├─ Next button → jumpToQuestion(qIndex + 1)
│  │  ├─ Prev button → jumpToQuestion(qIndex - 1)
│  │  └─ Click nav dot → jumpToQuestion(qIndex)
│  │
│  ├─ TIMER RUNNING
│  │  ├─ Every 1 second: timeLeft--
│  │  ├─ Update display: MM:SS
│  │  │
│  │  ├─ IF timeLeft == 300 (5 min)
│  │  │  └─ showNotification("5 minutes remaining!")
│  │  │
│  │  └─ IF timeLeft == 0
│  │     └─ endExam() (AUTO-SUBMIT)
│  │
│  └─ LOOP until user finishes or time up
│
├─ END EXAM
│  ├─ clearInterval(examTimer)
│  ├─ calculateExamScore(examAnswers, currentExam)
│  │  └─ Returns: {listening, reading, grammar, overall}
│  │
│  ├─ saveExamResult(scores)
│  │  ├─ Set lastExamDate = now (ISO)
│  │  ├─ Increment totalExamsTaken
│  │  ├─ Save scores: lastListeningScore, etc
│  │  ├─ Add to toeicHistory array
│  │  ├─ Award XP: floor(score / 10)
│  │  └─ localStorage.setItem('engportal_user', data)
│  │
│  └─ displayExamResults(scores)
│
├─ SHOW RESULTS
│  ├─ Display final score (0-990 TOEIC scale)
│  │
│  ├─ Assign grade:
│  │  ├─ >= 850 → "🌟 Outstanding!"
│  │  ├─ >= 750 → "🏆 Excellent!"
│  │  ├─ >= 600 → "👍 Good!"
│  │  ├─ >= 450 → "📖 Fair"
│  │  └─ < 450 → "💪 Keep Learning!"
│  │
│  ├─ Show breakdown:
│  │  ├─ 📻 Listening: X/250
│  │  ├─ 📖 Reading: X/250
│  │  └─ ✍️ Grammar: X/250
│  │
│  ├─ IF score >= 700
│  │  ├─ Generate & show certificate
│  │  ├─ Add "Download Certificate" button
│  │  └─ printCertificate() on click
│  │
│  └─ Show navigation buttons
│     ├─ "Take Next Exam" → retakeExam()
│     └─ "Back to Main" → backToExamWelcome()
│
└─ TRIGGER 30-DAY LOCK
   └─ Next exam attempt → checkExamAvailability()
      └─ Will show: "Come back in X days"
```

---

## 🔐 Security Flows

### XP Threshold Protection
```
User attempts exam
    ↓
checkExamAvailability() called
    ↓
if (user.xp < 500)
    ├─ return { canTakeExam: false, reason: "XP Threshold" }
    │
    └─ UI Layer:
       ├─ Disable button
       ├─ Show warning
       ├─ Calculate: 500 - user.xp
       └─ Suggest Daily Challenge
```

### 30-Day Lock Protection
```
User took exam TODAY (May 10)
    ↓
localStorage stores:
    lastExamDate = "2025-05-10T14:30:00Z"
    
User tries again (May 15)
    ↓
checkExamAvailability() called
    ↓
Calculate: May 15 - May 10 = 5 days
    ↓
if (5 < 30)
    ├─ return { daysUntilAvailable: 25 }
    │
    └─ UI Layer:
       ├─ Disable button  
       ├─ Show: "Available in 25 days"
       └─ User must wait
       
User tries again (June 9)
    ↓
Calculate: June 9 - May 10 = 30 days
    ↓
if (30 >= 30)
    ├─ return { canTakeExam: true }
    │
    └─ UI Layer:
       ├─ Enable button
       └─ User can take new exam
```

### Console Bypass Detection
```
User opens DevTools (F12)
    ↓
protectExamFromBypass() registered
    ↓
User tries: examBypassAttempt = true
    ↓
Trap activated
    ├─ Show warning notification
    ├─ Log to console: "Unauthorized attempt"
    └─ System continues normal operation
       (Protection is non-blocking)
```

---

## 📊 Data Flow Diagram

```
┌──────────────────┐
│  User Interaction│
└────────┬─────────┘
         │
         ▼
┌──────────────────────────────┐
│  Event Handler (click, etc)  │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Business Logic Function     │
│  (e.g., selectExamAnswer)    │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Update User State (examA)   │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Call Display Function       │
│  (e.g., displayQuestion)     │
└──────────┬───────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Render to DOM               │
│  Update UI Visual            │
└──────────────────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Periodic Save to Storage    │
│  localStorage update         │
└──────────────────────────────┘
           │
           ▼
┌──────────────────────────────┐
│  Data Persisted              │
└──────────────────────────────┘
```

---

## 📱 Mobile Responsive Breakpoint

```
Desktop (> 768px)
├─ Sidebar visible (280px)
├─ Main content: full width - sidebar
├─ Exam options: 3 columns
└─ Navigation dots: all visible

Mobile (≤ 768px)
├─ Sidebar hidden (slide out)
├─ Main content: full width
├─ Burger menu: visible
├─ Exam options: 1 column (stacked)
├─ Navigation dots: scrollable
└─ Font sizes: adjusted for readability
```

---

## 🧪 Testing Checkpoints

```
Test Sequence:
1. LOGIN
   ├─ Enter name → saved to localStorage
   └─ Verify profile updates

2. DAILY CHALLENGE
   ├─ Complete 25 days → 500 XP
   ├─ Verify XP counter increments
   └─ Check localStorage update

3. TOEIC BUTTON
   ├─ Initially DISABLED (XP < 500)
   ├─ After 500 XP → ENABLED
   └─ Click Start

4. EXAM TAKING
   ├─ Listening → Play audio
   ├─ Reading → Read passage
   ├─ Grammar → Select option
   ├─ Navigate questions
   └─ Timer counts down

5. COMPLETION
   ├─ Calculate scores
   ├─ Show breakdown
   ├─ Generate certificate (if score >= 700)
   └─ Save to localStorage

6. SECOND ATTEMPT
   ├─ Button DISABLED (30-day lock)
   ├─ Countdown shows
   ├─ After 30 days → ENABLED
   └─ New random questions generated
```

---

## 🎯 Component Status

| Component | Status | Tests |
|-----------|--------|-------|
| XP Threshold | ✅ Complete | Manual |
| 30-Day Lock | ✅ Complete | Manual |
| Question DB | ✅ Complete | Data validation |
| Randomizer | ✅ Complete | Logic verify |
| Web Speech | ✅ Complete | Browser support |
| Scoring | ✅ Complete | Math verify |
| Certificate | ✅ Complete | Print test |
| Protection | ✅ Complete | Console test |

---

**Architecture Document Version**: 1.0
**Last Updated**: May 2026
**Status**: Ready for Review

