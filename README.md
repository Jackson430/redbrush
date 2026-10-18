# RedBrush 朱笔

**The AI red pen for Mother Tongue teachers.** 出卷、批改、反馈，一笔完成。

An AI-powered assessment platform for **Singapore primary school Chinese (P1–P6)**. Teachers build a complete exam in minutes with Google Gemini; students take it online and get instant, rubric-based feedback; teachers review and override AI grades.

**Live demo:** _<paste your Firebase Hosting URL here>_
**Teacher password (for judges):** `huacheng2026`

## The problem
Chinese teachers in Singapore spend hours writing level-appropriate questions and marking open-ended answers by hand. Students wait days for feedback, and free-text Chinese answers are hard to grade automatically.

## What it does
**Teachers**
- Pick grade (P1–P6) and difficulty; Gemini adapts vocabulary and depth to that level.
- Type one target word and Gemini generates a full question in 7 formats: Hanyu Pinyin, word choice, word meaning, word usage, sentence completion, synonyms, and cloze.
- Reading comprehension: enter vocabulary and length, Gemini writes the passage and title, then generates sub-questions in 9 categories (inference, cause & effect, theme, character analysis, etc.) with answer keys or marking rubrics.
- Keeps correct answers from clustering on the same letter.
- Review every submission, edit scores and comments; teacher edits are marked as human comments.

**Students**
- Enter name, pick an exam, answer, submit.
- Multiple-choice is marked instantly; open-ended answers are graded by Gemini against the teacher's rubric, with a short comment.
- A 6-character receipt ID lets students reopen their results later.

## How we use Google Cloud
| Component | Role |
|---|---|
| **Gemini** (Gemini API) | Generates passages, titles, questions, options, rubrics; grades open-ended Chinese answers against rubrics with feedback comments |
| **Cloud Firestore** | Stores exams (`exams` collection) and student submissions (`submissions` collection), so data persists across devices and sessions |
| **Firebase Hosting** | Serves the web app globally over HTTPS |
| **Cloud Run** | Roadmap: move Gemini calls and grading server-side (see below) |

## Run / deploy
1. Open `public/index.html` and fill in `window.APP_CONFIG` at the top: Gemini API key and your `firebaseConfig`.
2. In Firebase console, create a Firestore database.
3. Deploy:
   ```bash
   npm install -g firebase-tools
   firebase login
   firebase deploy --only hosting --project YOUR_PROJECT_ID
   ```

## Roadmap
- Move Gemini calls and answer checking into a **Cloud Run** service so API keys and answer keys never reach the browser.
- **Firebase Authentication** for teacher accounts.
- Gemini multimodal grading of **photographed handwritten answers**.
- Extend to Singapore's other Mother Tongue languages (Malay, Tamil).

## Prototype limitations (honest notes)
This prototype was built under hackathon time limits. The Gemini key is called from the browser (restrict it by HTTP referrer in Google Cloud console), the teacher password is a shared demo password, and Firestore uses open test-mode rules.
