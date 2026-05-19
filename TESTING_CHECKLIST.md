# EngPortal TOEIC System v2.0 - Testing Checklist

## ✅ Implementation Verification

### Database & Data Structure
- [x] `toeicDatabase` dengan 25+ soal
  - [x] 5 Listening soal
  - [x] 5 Reading soal
  - [x] 10 Grammar soal
- [x] User object extended dengan fields baru
- [x] Backward compatibility untuk old exam records

### Core Functionality
- [x] `checkExamAvailability()` - Check 500 XP & 30-day lock
- [x] `generateRandomExam()` - Random 12 soal
- [x] `speakQuestion()` - Web Speech API integration
- [x] `calculateExamScore()` - Score breakdown per section
- [x] `saveExamResult()` - Save to localStorage
- [x] `protectExamFromBypass()` - Console protection

### UI/UX Implementation
- [x] `initExamSection()` - Initialize exam screen
- [x] `updateExamUI()` - Display availability status
- [x] `startExam()` - Start dengan availability check
- [x] `displayExamQuestion()` - Show Q dengan listening support
- [x] `selectExamAnswer()` - Save answer
- [x] `endExam()` - Auto-end saat waktu habis
- [x] `displayExamResults()` - Show breakdown + certificate
- [x] `renderTOEICChart()` - History visualization

### Event Listeners
- [x] Start button click handler
- [x] Next/Previous navigation
- [x] Question nav dots click
- [x] Answer selection
- [x] Retake exam button

---

## 🧪 Manual Testing Procedures

### Test Case 1: XP Threshold Lock
```
SETUP:
- Buat user baru (Guest)
- Verify XP = 0

TEST:
1. Navigate ke "Monthly TOEIC"
2. Check: startExamBtn should be DISABLED
3. Check: examStatus shows "XP Threshold Required"
4. Check: "Need 500 more XP" message

PASS CRITERIA:
- Button disabled dengan opacity 0.5
- Warning message tampil
- Current XP ditampilkan
```

### Test Case 2: XP Accumulation
```
SETUP:
- User memiliki 0 XP

TEST:
1. Navigate ke "Daily Challenge"
2. Complete Daily Challenge (answer correctly)
3. Verify: +20 XP awarded
4. Repeat 25x times untuk kumpulkan 500 XP
5. Navigate ke "Monthly TOEIC"
6. Check: startExamBtn now ENABLED

PASS CRITERIA:
- XP counter updates correctly
- Button enabled setelah 500 XP tercapai
- UI responsive
```

### Test Case 3: Start Exam Flow
```
SETUP:
- User has >= 500 XP
- Belum pernah atau sudah lewat 30 hari

TEST:
1. Click "Start Exam"
2. Verify: examWelcome hidden
3. Verify: examActive visible
4. Verify: First question displayed
5. Verify: Timer starts (45:00)
6. Verify: Question nav dots visible

PASS CRITERIA:
- Correct section visible
- Timer running
- All elements rendered
```

### Test Case 4: Listening Section
```
SETUP:
- Exam in progress
- On listening question

TEST:
1. Observe: "🔊 Play Audio" button visible
2. Click: Play Audio button
3. Verify: Audio plays (browser speaks)
4. Verify: currentlyPlayingAudio = true during play
5. Verify: Can select answer after audio
6. Select answer
7. Click Next

PASS CRITERIA:
- Audio plays without error
- UI remains responsive
- Answer saved correctly
- Navigation works
```

### Test Case 5: Reading Section
```
SETUP:
- Navigate to Reading question

TEST:
1. Verify: Passage displayed with border (--neon-purple)
2. Verify: Question below passage
3. Select answer
4. Verify: Selected option highlighted
5. Click Next

PASS CRITERIA:
- Passage clearly visible
- Question formatted correctly
- Answer selection works
```

### Test Case 6: Grammar Section
```
SETUP:
- Navigate to Grammar question

TEST:
1. Verify: Question with blank (___)
2. Verify: 4 options A, B, C, D
3. Select correct option
4. Verify: Highlighted

PASS CRITERIA:
- Question formatted correctly
- Options labeled A-D
- Selection works
```

### Test Case 7: Navigation & Timing
```
SETUP:
- In exam

TEST:
1. Move between questions using nav dots
2. Verify: Current dot highlighted (gold)
3. Verify: Answered dots blue
4. Click Previous/Next buttons
5. Verify: Correct navigation
6. Let timer count down to 5:00
7. Verify: Warning notification shows

PASS CRITERIA:
- Navigation smooth
- Dots update correctly
- Timer warning appears
```

### Test Case 8: Exam Completion
```
SETUP:
- Complete exam or timer hits 0:00

TEST:
1. All answers submitted
2. Verify: examActive hidden
3. Verify: examResults visible
4. Verify: Final score displayed
5. Verify: Grade text displayed
6. Verify: Score breakdown visible
   - Listening: X/250
   - Reading: X/250
   - Grammar: X/250

PASS CRITERIA:
- Results page formatted correctly
- Scores calculated accurately
- Breakdown clear
```

### Test Case 9: Certificate (Score >= 700)
```
SETUP:
- Complete exam with score >= 700

TEST:
1. Verify: Certificate displayed
2. Verify: User name in certificate
3. Verify: Score in certificate
4. Click: "Download Certificate"
5. Verify: Print dialog opens

PASS CRITERIA:
- Certificate visible
- Print function works
- All info correct
```

### Test Case 10: 30-Day Lock
```
SETUP:
- User just completed exam

TEST:
1. Navigate to Monthly TOEIC immediately
2. Verify: startExamBtn DISABLED
3. Verify: "Monthly Cooldown Active" message
4. Verify: Days countdown displayed
5. Check localStorage: lastExamDate set to today

PASS CRITERIA:
- Button disabled
- Countdown correct
- Message clear
```

### Test Case 11: Data Persistence
```
SETUP:
- User complete exam with score 650

TEST:
1. Check localStorage:
   ```javascript
   const user = JSON.parse(localStorage.getItem('engportal_user'));
   console.log(user.lastExamDate);
   console.log(user.toeicHistory);
   console.log(user.bestTOEIC);
   ```
2. Reload page
3. Verify: Data persisted
4. Navigate to Profile
5. Verify: Exam history visible

PASS CRITERIA:
- lastExamDate is ISO string
- toeicHistory has entry
- bestTOEIC updated
- Data survives reload
```

### Test Case 12: Console Protection
```
SETUP:
- Open browser console

TEST:
1. Try: `examBypassAttempt = true`
2. Verify: Warning notification shows
3. Verify: Console shows warning log
4. Try: Access `currentExam` directly
5. Verify: Cannot manipulate state easily

PASS CRITERIA:
- Protection function works
- Warnings displayed
- No easy bypass possible
```

---

## 🔧 Performance Checklist

- [ ] Page load time < 2s
- [ ] Exam transitions smooth
- [ ] No lag when clicking options
- [ ] Timer accurate (±1 second)
- [ ] Audio playback smooth
- [ ] No console errors
- [ ] Mobile responsive (768px breakpoint)

---

## 📱 Cross-Browser Testing

- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Chrome (Android)
- [ ] Mobile Safari (iOS)

**Note**: Web Speech API support varies by browser

---

## 🔐 Security Verification

- [ ] XP threshold properly enforced
- [ ] 30-day lock working
- [ ] localStorage encryption (browser default)
- [ ] No XSS vulnerabilities
- [ ] No localStorage injection possible
- [ ] Console bypass detected
- [ ] Data validation on load

---

## 📊 Data Integrity

- [ ] Old exam records migrate correctly
- [ ] New fields have defaults
- [ ] Scores calculated accurately
- [ ] Dates in ISO format
- [ ] No NaN values
- [ ] Grade assignment correct

---

## ✨ Final Sign-off

**QA Approved**: [ ]
**User Testing**: [ ]
**Production Ready**: [ ]

**Date**: ___________
**Tester**: ___________
**Notes**: ___________

---

## 🚀 Deployment Checklist

- [ ] All tests passed
- [ ] No console errors
- [ ] Mobile responsive tested
- [ ] Browser compatibility verified
- [ ] Database population confirmed
- [ ] Documentation complete
- [ ] User guide reviewed
- [ ] Backup of original file created
- [ ] Ready for production deployment

---

**Version**: 2.0
**Last Updated**: May 2026
**Status**: Ready for Testing Phase

