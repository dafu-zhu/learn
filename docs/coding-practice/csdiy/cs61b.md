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

- [ ] 1/22 - 1/24:  Attend lab 1.
- [ ] 1/24: Submit lab 1. Note: If you join the class late or just miss the deadline, you can request an extension at https://fa25.beacon.datastructur.es
- [ ] 1/24: Complete HW0A. Note: If you join the class late or just miss the deadline, you can request an extension at https://fa25.beacon.datastructur.es/

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