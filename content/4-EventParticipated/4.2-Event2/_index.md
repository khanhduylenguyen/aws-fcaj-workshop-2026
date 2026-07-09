---
title: "Event 2 – Cloud Architect – AWS Knowledge Tournament"
date: 2026-06-20
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Event Report: "Cloud Architect" Tournament — from the spectator seats

## 1. Event Overview

| | |
|---|---|
| **Event Name** | Cloud Architect (AWS knowledge tournament) |
| **Date & Time** | June 20, 2026 |
| **Venue** | 26th Floor, Bitexco Financial Tower, 02 Hai Trieu Street, District 1, HCMC |
| **Organizer** | First Cloud AI Journey (FCAJ) community |
| **My Role** | Audience / spectator watching the competing teams |

## 2. About the Tournament

"Cloud Architect" is a head-to-head knowledge tournament between teams of Cloud learners/engineers, organized by the **FCAJ community** on the 26th floor of Bitexco. The format is designed to stress-test three dimensions:

- **Reflex speed** when facing AWS questions under time pressure.
- **System-design thinking** through real-world scenarios.
- **Breadth and depth of AWS knowledge** — from foundational (Cloud Practitioner) to expert (SAP).

I came to the event as a **spectator** — sitting in the back rows, watching the matches, listening to the MC, and reacting with the rest of the audience. Even though I didn't hold the answer board, my heart still raced on every question — especially during the quarter-final match I'll recap below.

## 3. Tournament Rules (transcribed from the MC's announcement)

### 3.1. Points by Certification Level

Each question is pre-tagged with a level matching an AWS certification. The harder the level, the higher the points:

| Certification Level | Points per Correct Answer |
|---|---|
| **AWS Cloud Practitioner** (foundational) | 10 pts |
| **AWS Solutions Architect Associate (SAA)** | 20 pts |
| **AWS Solutions Architect Professional (SAP)** | 50 pts |

### 3.2. Three-Round Structure

Every round has 10 questions, with the proportion shifting toward harder levels as the tournament advances:

| Round | Practitioner | SAA | SAP |
|---|---|---|---|
| **Qualifying (Quarter-finals)** | 7 questions | 2 questions | 1 question |
| **Semi-finals** | 5 questions | 3 questions | 2 questions |
| **Finals** | 3 questions | 4 questions | 3 questions |

### 3.3. Two Strategic Lifelines

Each team enters with **2 high-stakes lifelines**:

| Lifeline | Scoring Rule | When to use |
|---|---|---|
| **Minimum Risk** | Correct → +50% of the points. Wrong → no penalty. | Safe play on moderate questions. |
| **Star of Hope** | Correct → double the points. Wrong → deduct double. | High-risk "all-in" on hard questions. |

## 4. The Match I'll Remember: PrimeOps vs KLKÂT (Quarter-final)

I watched several matches throughout the day, but **the quarter-final between PrimeOps and KLKÂT** was the one that silenced the whole room. Here's how it unfolded from my seat in the audience.

### 4.1. The atmosphere before the match

When the MC called the two teams onto the stage, the venue was packed — mostly FCAJ learners like me, plus guests and supporters. The two teams had very different vibes: **PrimeOps** was calm, with their answer boards always set neatly on the desk; **KLKÂT** was more animated, constantly discussing strategy before each question.

### 4.2. The first 9 questions — a "one-sided" lead for PrimeOps

For the first 9 questions (7 Practitioner + 2 SAA), I was already thinking *"this match is over"*. PrimeOps was almost flawless:

- They nailed the foundational and associate-level questions.
- Their buzzer speed was noticeably faster than KLKÂT.
- The gap between the two teams grew to **~70 points** before the final question.

The audience was already mentally preparing for PrimeOps to move on. So was I.

### 4.3. The last question — SAP 50 points, and the comeback

| Aspect | What happened (from the spectator's view) |
|---|---|
| **Situation** | PrimeOps leading by 70 points — a ticket to the next round almost in hand. |
| **KLKÂT's decision** | After the MC read the SAP prompt, KLKÂT's captain asked for 30 extra seconds, then declared: *"Go big or go home"* — activating **Star of Hope** on the SAP 50-point question. The whole room let out a small gasp. |
| **Both teams' reactions** | Both teams looked at each other and forced a smile. The question was clearly deep — something around advanced networking or hybrid architecture. From the outside, nobody looked 100% sure. |
| **The deciding moment** | After thinking time, KLKÂT locked in their answer first. PrimeOps deliberated a few more seconds, then picked a different option. The room went silent, waiting for the MC to flip the board. |
| **Result** | The MC flipped the board: **KLKÂT was correct**. The audience clapped, but you could also hear murmuring — it was obviously a question where *nobody* was really sure. The 100 points (50 × 2) wiped out the entire 70-point gap. KLKÂT completed the comeback. PrimeOps stood up, shook hands with their opponents, and stepped off the stage. |

Sitting in the back rows, I could really feel how all the drama of the match was concentrated in those final 10 minutes — the kind of moment I'll remember for a long time.

## 5. Lessons I Took Away as a Spectator

Even without holding the answer board, watching from the outside gave me a much bigger picture.

### 5.1. Luck is part of the game (and reality)

Watching from the audience, it became obvious: in any Cloud system, no matter how well you prepare (just like PrimeOps' 70-point lead), **unforeseen variables** are always there:

- Zero-day vulnerabilities appearing out of nowhere.
- Natural-disaster incidents hitting a Data Center.
- Or simply a competitor's *"right-place-right-time"* decision.

The takeaway: the goal isn't to eliminate luck — it's to **design systems that survive even when luck goes against you**. That's exactly what AWS HA/DR patterns do (multi-AZ, multi-region, pilot light) — the workload keeps running even when individual components fail.

### 5.2. Risk Management — viewing the chessboard

KLKÂT's all-in bet is a textbook example of **accepting extreme risk in exchange for outsized upside**. In **AWS architecture design**, the same dilemma always exists:

- **Safe but costly:** multi-region active-active, 3-AZ with near-zero RTO.
- **Reckless but cheap:** single-region, pilot light, backup-only.
- There's no universal formula — only **trade-offs between risk and benefit**, decided by the business context.

I told myself: just as you shouldn't play "Star of Hope" on every question, you shouldn't run multi-region for every workload either. Use it only when the business case truly demands it.

### 5.3. Never Stop Learning — knowledge gaps spare no one

That final SAP 50-point question froze both teams — and definitely me too. It was a sharp reminder that **AWS knowledge is endless**. The best way to reduce dependency on luck is to **keep closing your own knowledge gaps** — especially the corners the workshops rarely touch: advanced networking, hybrid architectures, cost optimization at enterprise scale.

## 6. Personal Reflection from the Spectator Seats

Even as a spectator, I went home with a lot of emotions:

- **Real tension:** from the back rows, I was on the edge of my seat for every question — especially the last 10 of that quarter-final. A classmate next to me even live-streamed the whole match to our study group.
- **Respect for KLKÂT's all-in call:** they knew exactly what they were doing and accepted the risk for the upside. That's the composure of a real competitive team.
- **Admiration for PrimeOps' sportsmanship:** even after losing at the very last moment, they immediately stood up and shook hands with their opponents — no excuses, no arguments. That's the most valuable lesson of the day.
- **Personal motivation:** watching the teams play, I could see my own knowledge gaps. I've set myself a near-term goal of earning the **AWS Solutions Architect Professional (SAP-C03)** certification.

## 7. Some Photos from the Event

*Add your event photos here*

> Sitting in the spectator seats taught me one thing: sometimes the deepest lessons come from observing. An AWS knowledge tournament isn't just a contest of knowledge — it's a miniature lesson in **risk, luck, and strategic thinking** — all the things I'll face again every day when running real Cloud systems in production.
