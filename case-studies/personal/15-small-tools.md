# 15 · Small tools

Breadth matters too. Smaller things I have built, each in one or two sittings:

| Tool | What it does | Notes |
|---|---|---|
| Budget app | Single-page personal budget tracker (public repo) | No backend, one HTML file |
| Used-car DMS | Single-user used-car management web app: React/Vite, Vercel, Supabase | Slice 1 runs on demo data from a spreadsheet so the UI can be judged before wiring the database |
| Batch PDF printer | Sends many PDFs to a printer through a bundled PDF viewer | PyInstaller `.exe` for non-technical users |
| Browser bridge | Local OpenAI-compatible endpoint backed by a browser tab | See case 14 |
| Remote-access notes | Documented a remote Claude Code setup between two work machines | Kept the *original plan* and the *real setup log* separate, and marked what the plan got wrong (the server was Windows with WSL, and I had no admin rights) |
| Interview / job-hunt site | Preparation notes and resume as a static site | |
| Video-editing projects | Editing project files, kept in a private repo | |

## Pattern
Start from a demo-data slice, show it, then connect the real backend. Document the real state of
a setup in a log, not the hoped-for plan.
