<div align="center">

[中文](README.md) | **English**

</div>

# Lawyer Case Ledger Intake Skill

Turn a lawyer's plain-language case description into structured fields and write them back to a Kingsoft (WPS) multi-dimensional table. Covers all 7 stages (Retention / Filing / Hearing / Preservation / Judgment / Execution / Closure), with automatic limitation-of-actions calculation, gap-checking, and a minimal single-table output.

> Output is a lawyer's working draft, not legal advice.

## Author & Customization

This skill and the ledger template are developed by **Attorney Chen Heng (陈恒律师)** (GitHub: [@lawyerCH](https://github.com/lawyerCH)) for personal use and published openly — free to duplicate. Need more features (email notifications, WeChat/Feishu integration, automated reminders, etc.)? Open an Issue in this repository to contact the author for customization.

## One-Click Install & Use (copy-paste to AI)

> Install the skill from https://github.com/lawyerch-law/case-ledger-intake into ~/.workbuddy/skills/case-ledger-intake (skip if already installed); complete first-time binding per SKILL.md: first check the kdocs connector, guide me to enable it if not connected; if I don't have a ledger, duplicate the author's template from https://www.kdocs.cn/l/cubUxHzlKoTl into the root of my Kingsoft cloud drive (skip if I already have one). **Stop once binding is done and wait for my instructions — do not log cases on your own.**

## Manual Installation (3 steps)

**① Connect Kingsoft Docs**: enable and authorize the **kdocs** connector in WorkBuddy's connector manager.

**② Duplicate the author's ledger template** (required for new users; skip if you already have a ledger):
- Option A (AI-assisted): tell AI "duplicate《律师个人案件管理台账》from https://www.kdocs.cn/l/cubUxHzlKoTl into my cloud drive and bind it"
- Option B (manual): open the link above in a browser → "Save to my cloud drive" → tell AI once done

**③ Install the skill**:

```bash
git clone https://github.com/lawyerch-law/case-ledger-intake.git ~/.workbuddy/skills/case-ledger-intake
ls ~/.workbuddy/skills/case-ledger-intake/SKILL.md   # verify structure
```

**Prerequisites**: WorkBuddy + kdocs connector connected + a Kingsoft multi-dimensional ledger (new users duplicate the author's template per ②; existing users use their own).

**Auto-binding**: on first use, AI searches for your ledger — found → binds directly; not found → follows the "template duplication" path (AI copies from the author's template via `save_as_file` into your drive) → generates local `config.json` (private, not committed) → resolves tables/fields/views → starts intake.

## Usage Examples (one input per stage)

### Example 1 | Retention Stage (new case)

**You say**: Help me log a new case. My friend Mr. X lent 300,000 CNY to Mr. Y, promissory note signed January 2023, repayment due end of June 2023 — nothing repaid, phone unreachable. I want to sue on his behalf. Fee: 10,000 CNY upfront, retainer contract not yet signed.

**AI will**:
1. Identify retention stage → parse: plaintiff Mr. X, defendant Mr. Y, cause "private lending dispute" (inferred), first instance (inferred)
2. **Calculate limitation period**: due 2023-06-30 + 3 years = expires 2026-06-30 → ⚠️ expired; ask about interruption evidence
3. You answer "WeChat demand on 2025-01-03" → interruption resets expiry to **2028-01-02**, risk cleared
4. Confirm inferred items → write to main table + fee record → verify → recap

### Example 2 | Filing Stage (backfill + response deadlines)

**You say**: Mr. X's sales contract case is filed. XX District Court, case no. (2026)粤01民初XXXX号, filed Aug 30. Defendant's answer period until Sep 18, evidence period until Sep 20. Court fee paid, materials complete.
(When we are the defendant: we've been sued, just received the complaint, hearing scheduled Oct 10 — AI pins "⚠️ URGENT: answer period / hearing date", first confirms the court and whether the answer period has passed, prompts immediate case review/answer)

**AI will**: backfill case number/filing date/court → **register deadline reminders** (answer Sep-18, evidence Sep-20; reminder date filled directly) → action checklist (jurisdiction check / link materials / court fee) → write back.

### Example 3 | Hearing Stage (hearing info)

**You say**: Summons received for case X — Oct 10, 9:30 AM, Courtroom 3, XX District Court, Judge Wang, tel 12345678.

**AI will**: tick "hearing held" → log hearing details (time/venue/session/phone) into the **hearing record sub-table** → register a reminder (type "Other", note "hearing summons") → write back.

### Example 4 | Preservation Stage (asset preservation)

**You say**: Case X — preservation granted, Mr. Y's bank account frozen 500,000 CNY by XX District Court, preservation ruling received.

**AI will**: fill preservation status/amount/authority → register preservation and renewal reminders (watch 30 days before expiry) → link the ruling under "Materials received" → write back.

### Example 5 | Judgment Stage (judgment result)

**You say**: Judgment in case X — received Oct 8, Mr. Y ordered to pay 300,000 CNY plus interest.

**AI will**: fill judgment/service date Oct-08 → calculate appeal deadline (service date +15 days) → register reminder → confirm with client whether to appeal → write back.

### Example 6 | Execution Stage (enforcement application)

**You say**: Judgment is final, the other party hasn't performed — preparing to apply for enforcement.

**AI will**: verify performance deadline and **enforcement application period** (maturity +2 years) → fill enforcement fields → register reminder → write back.

### Example 7 | Closure Stage (close & archive)

**You say**: Case X enforcement complete — closing today, archiving.

**AI will**: fill closing date → wrap up case logs/documents → mark closure status → write back.

## Directory Structure

```
case-ledger-intake/
├── SKILL.md            # Main spec: workflow/rules/output (single source of truth)
├── README.md           # This file (Chinese)
├── README_EN.md        # English version
├── SHARE.md            # Distribution safety guide (read before re-distributing)
├── lawyer-profile.md   # Lawyer profile (personal habits; fill in yourself; clear before committing)
├── config.json         # ⚠️ auto-generated locally, never committed
└── references/         # 7-stage guides + ledger structure/sub-table reference
```

## Privacy

Repository content is anonymized (no real file_id / links / parties / case numbers / amounts / names). `config.json` and `lawyer-profile.md` are personal data and are not committed; follow the `SHARE.md` checklist before forking or re-distributing.

## Disclaimer

This skill and ledger are the author's personal-use templates, designed around the author's own case-handling needs, not a commercial product. Users should evaluate and adapt them to their own workflows; the author accepts no responsibility for any issues arising from use.

## License

[MIT](LICENSE) — Copyright (c) 2026 Chen Heng (陈恒)
