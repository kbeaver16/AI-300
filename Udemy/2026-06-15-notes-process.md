# Notes Process

**Date:** 2026-06-15  
**Tags:** #notes #AI-300 

---

## 🧠 Thoughts
-  

---

## 🔍 Details
### Process to get notes for Udemy lectures:
1. Use Smart Audio Capture plugin to record lecture
2. Use `whisper-cli` to transcribe mp3 file
```bash
whisper-cli -m models/base.en.bin -f <fileName> -otxt
```
3. Give the transcription file to Copilot and have it generate the notes for the lecture
```Copilot
Turn this transcript into structured Obsidian notes with:
Key concepts
Definitions
Examples
Step‑by‑step processes
Code blocks (if any)
A summary
Actionable takeaways
Tags
Backlink suggestions
Make the results a markdown file I can copy copy into Obsidian
```

---

## ➡️ Next Steps

