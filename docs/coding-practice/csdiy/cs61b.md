---
title: CS61B
parent: CS DIY
nav_order: 1
toc: true
---

# CS 61B
{: .no_toc}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Checklist

**Today's actual date:** <span id="actual-date">Loading...</span>  
**Corresponding course day:** <span id="course-date">Loading...</span>

Week 1 (no discussion sections)

- [x] 1/22 - 1/24:  Attend lab 1.
- [x] 1/24: Submit lab 1. Note: If you join the class late or just miss the deadline, you can request an extension at https://fa25.beacon.datastructur.es
- [ ] 1/24: Complete HW0A. Note: If you join the class late or just miss the deadline, you can request an extension at https://fa25.beacon.datastructur.es/

Week 2 (discussion sections start)
- [ ] 1/29: Complete HW0B.
- [ ] 1/27: Begin work on Mini-Project 0 (2048).
- [ ] 1/27: Complete Week 2 Survey (part of course grade; feedback on week of 1/22 - 1/24)
  - [ ] Note: There is no week 1 survey. 
- [ ] 1/31: Submit lab 2.
- [ ] 1/31: Pre-semester Survey (2.5 pts of ec) 

Week 3
- [ ] 2/3: Complete and submit Project 0 (2048). 
- [ ] 2/3: Start Project 1A (LinkedListDeque).
- [ ] 2/3: Complete Week 3 Survey (part of course grade; feedback on week of 1/27 - 1/31)
- [ ] 2/7: Complete and submit Homework 1.
- [ ] 2/7: Submit lab 3.

Week 4:
- [ ] 2/10: Submit Project 1A (LinkedListDeque). 
- [ ] 2/10: Complete Week 4 Survey (part of course grade; feedback on week of 2/3 - 2/7)
- [ ] 2/11: Start Project 1B (ArrayDeque, Deque).
- [ ] 2/14: Submit lab 4.

Week 5:
- [ ] 2/18: Complete Week 5 Survey (part of course grade; week of 2/10 - 2/14)
  - [ ] Note: 2/17 is President’s Day 
- [ ] 2/19: Submit Project 1B (ArrayDeque, Deque).
- [ ] 2/20: Midterm 1 (primarily tests work from Project 0 and Project 1).


---

<script>
window.addEventListener('load', function() {
  const myStart = new Date(2025, 10, 22);
  const courseStart = new Date(2025, 0, 22);
  const offsetMs = courseStart - myStart;
  const today = new Date();
  const courseToday = new Date(today.getTime() + offsetMs);
  const options = { month: 'numeric', day: 'numeric', year: 'numeric' };
  document.getElementById('actual-date').textContent = today.toLocaleDateString('en-US', options);
  document.getElementById('course-date').textContent = courseToday.toLocaleDateString('en-US', options);
});
</script>