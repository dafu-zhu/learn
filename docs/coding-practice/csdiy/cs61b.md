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

**Today's actual date:** <span id="actual-date"></span>  
**Corresponding course day:** <span id="course-date"></span>

<script>
(function() {
  // Your actual start date (Nov 22, 2025)
  const myStart = new Date(2025, 10, 22); // Month is 0-indexed, so 10 = November
  
  // Course start date (Jan 22, 2025)
  const courseStart = new Date(2025, 0, 22); // 0 = January
  
  // Calculate offset in days
  const offsetMs = courseStart - myStart;
  
  // Get today's date and apply offset
  const today = new Date();
  const courseToday = new Date(today.getTime() + offsetMs);
  
  // Format and display
  const options = { month: 'numeric', day: 'numeric', year: 'numeric' };
  document.getElementById('actual-date').textContent = today.toLocaleDateString('en-US', options);
  document.getElementById('course-date').textContent = courseToday.toLocaleDateString('en-US', options);
})();
</script>

- [ ] 1/22 - 1/24:  Attend lab 1.
- [ ] 1/24: Submit lab 1. Note: If you join the class late or just miss the deadline, you can request an extension at https://fa25.beacon.datastructur.es
- [ ] 1/24: Complete HW0A. Note: If you join the class late or just miss the deadline, you can request an extension at https://fa25.beacon.datastructur.es/
