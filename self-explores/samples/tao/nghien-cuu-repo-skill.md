---
name: "Nghien cuu & Phan tich Repo Skill"
description: "Template nghien cuu repo skill: phan tich tong quan, xep hang, deep dive tung skill, review tong hop. Dung khi bat dau hoc/trien khai mot repo skill moi."
type: tao
tags: [research, skill-analysis, learning, repo-audit]
created: 2026-03-25
updated: 2026-03-25
used_count: 1
---

# Mau: Nghien cuu & Phan tich Repo Skill

## Mo ta
Dung khi bat dau nghien cuu mot repository chua cac skill definitions (file .md, prompt files).
Workflow: Phan tich tong quan -> Xep hang uu tien -> Deep dive tung skill -> Tong hop review.

Tieu chi danh gia theo Anthropic Standards:
- **Do day du (Completeness):** Bao quat edge cases?
- **Tinh tien dung (Usability):** De dung cho nguoi moi?
- **Tinh module hoa:** De tach roi/tai su dung?
- **An toan & Tin cay:** Theo quy tac prompt engineering cua Anthropic.

## Tasks

### Task 1: Phan tich Tong quan
- **Type:** task
- **Priority:** P0
- **Estimate:** 45 phut
- **Description:** |
  Liet ke toan bo skill co trong repo {repo_name}.
  Voi moi skill: ten, file path, mo ta 1 dong, linh vuc (domain).
  Phan nhom theo linh vuc (VD: coding, workflow, ML, deployment...).
  Output: bang tong hop + so luong skill theo nhom.
  Luu ket qua vao self-explores/learn/_overview.md
- **Dependencies:** none
- **Acceptance Criteria:**
  - Moi file .md / prompt file trong repo deu duoc liet ke
  - Phan nhom ro rang, khong trung lap
  - File _overview.md co bang tong hop day du

### Task 2: Xep hang uu tien
- **Type:** task
- **Priority:** P0
- **Estimate:** 30 phut
- **Description:** |
  Danh gia muc do quan trong (1-5) cho tung skill da liet ke o Task 1.
  Tieu chi xep hang:
  - Tinh ung dung thuc te (dung duoc ngay vs ly thuyet)
  - Do pho bien (nhieu nguoi can vs niche)
  - Do phuc tap (de hoc vs can thoi gian)
  - Gia tri leverage (hoc 1 lan, dung nhieu lan)
  Sap xep theo thu tu: nen hoc skill nao truoc, kem ly do.
  Luu vao self-explores/learn/_priority-ranking.md
- **Dependencies:** [Task 1]
- **Acceptance Criteria:**
  - Moi skill co diem 1-5 va ly do
  - Top 5 skill uu tien co giai thich cu the
  - File _priority-ranking.md co bang xep hang

### Task 3: Deep Dive — {skill_name}
- **Type:** task
- **Priority:** P1
- **Estimate:** 60 phut (moi skill)
- **Description:** |
  Nghien cuu sau skill "{skill_name}" (lap lai task nay cho moi skill quan trong).
  Phan tich:
  1. Logic core: Skill hoat dong nhu the nao? Cac buoc xu ly chinh?
  2. Danh gia chat luong (Anthropic Standards):
     - Completeness (1-10): Bao quat edge cases?
     - Usability (1-10): De dung cho nguoi moi?
     - Modularity (1-10): De tach roi/tai su dung?
     - Safety (1-10): An toan theo prompt engineering best practices?
  3. Uu & Nhuoc diem
  4. Kha nang tich hop vao he thong hien tai
  5. Ghi chu ky thuat (Technical Decisions)
  Output file: self-explores/learn/{skill_name}/analysis.md (theo format ben duoi)
- **Dependencies:** [Task 2]
- **Acceptance Criteria:**
  - File analysis.md du 5 sections
  - Diem danh gia co giai thich, khong chi ghi so
  - Ghi chu ky thuat cu the (khong chung chung)

### Task 4: Review & Notion
- **Type:** task
- **Priority:** P1
- **Estimate:** 30 phut
- **Description:** |
  Tong hop toan bo ket qua nghien cuu thanh bao cao cuoi.
  Gom:
  - Bang tong quan (tu Task 1)
  - Xep hang uu tien (tu Task 2)
  - Tom tat deep dive moi skill (tu Task 3)
  - Recommendations: Nen ap dung gi truoc? Lo trinh hoc tap?
  Chuan bi cho /viec review de day len Trello + Notion.
  Luu vao self-explores/learn/_final-report.md
- **Dependencies:** [Task 3]
- **Acceptance Criteria:**
  - Bao cao co du 4 phan tren
  - Recommendations co lo trinh cu the (tuan 1 lam gi, tuan 2 lam gi)
  - San sang cho /viec review

## Placeholders
Khi dung mau nay, thay the cac gia tri sau:
- `{repo_name}` — Ten repository can nghien cuu (VD: andrej-karpathy-skills)
- `{skill_name}` — Ten skill cu the khi tao task Deep Dive (lap lai cho moi skill)
- `{output_base}` — Thu muc goc luu ket qua (mac dinh: self-explores/learn/)

## Format file output (analysis.md)

Moi file trong `self-explores/learn/{skill_name}/` phai co:

```markdown
# {Ten Skill}

## Thong tin co ban
- **Repo:** {repo_name}
- **File path:** {duong dan file goc}
- **Domain:** {linh vuc}
- **Ngay phan tich:** {YYYY-MM-DD}

## Danh gia (Score 1-10)
| Tieu chi | Diem | Giai thich |
|----------|------|------------|
| Completeness | X/10 | {ly do} |
| Usability | X/10 | {ly do} |
| Modularity | X/10 | {ly do} |
| Safety | X/10 | {ly do} |
| **Tong** | **X/10** | |

## Uu diem
- {diem manh 1}
- {diem manh 2}

## Nhuoc diem
- {diem yeu 1}
- {diem yeu 2}

## Kha nang tich hop vao he thong hien tai
{Phan tich cu the: co the plug-in truc tiep khong, can chinh sua gi,
 xung dot voi component nao khong}

## Ghi chu ky thuat (Technical Decisions)
- {Quyet dinh thiet ke dang chu y}
- {Trade-offs da chon}
- {Patterns/anti-patterns phat hien}

## Takeaways
- {Dieu quan trong nhat rut ra}
- {Ung dung ngay duoc}
```

## Notes
- Task 3 (Deep Dive) can duoc lap lai nhieu lan — tao 1 task cho moi skill quan trong (top 5-10 tu Task 2)
- Neu repo lon (>20 skills): chi deep dive top 10, con lai ghi tom tat 2-3 dong trong _overview.md
- Nen chay /viec phanbien cho Task 1 va Task 2 truoc khi bat dau Deep Dive
- Flow khuyen nghi: tao tasks -> phanbien -> chitiet -> bat-dau -> xong -> review
