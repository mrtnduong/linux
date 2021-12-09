# CMPE 283 Assignment 4

Group Members: Martin Duong & Ruchit Patel

Martin Duong: Ran assignment with/without EPT
Ruchit Patel: Answered questions

Assumptions: Working Assignment 3

## Step-by-Step Instructions
![Steps](https://user-images.githubusercontent.com/2999334/144771357-65f95d75-b238-43f0-afa0-cf3a82c355d5.PNG)

## With EPT (Nested Paging)
![NestedPaging](https://user-images.githubusercontent.com/2999334/144771253-2d5d55ef-13d9-41e2-a5b1-e193df2308d0.PNG)

## Without EPT (Shadow Paging)
![ShadowPaging](https://user-images.githubusercontent.com/2999334/144771256-b29a9054-bcc1-4eba-95cc-a8490af4a522.PNG)

## Questions
1) What did you learn from the count of exits? Was the count what you expected? If not, why not?
  - We observed that the count of exits greatly increased without EPT (shadow paging). The count was what we expected since with shadow paging, the VMM needs to intervene and handle exits much more often as we need to turn on more exits, such as exit when there is a change in CR3 and exit on page fault. On the other hand, nested paging accesses the page table directly without the need to constantly exit.
2) What changed between the two runs (ept vs no-ept)?
  - Between the two runs, there were a couple of differences, first being that when booting up the VM without EPT (shadow paging), the bootup process was slower than with EPT (nested paging). Secondly, without EPT, we observed three exit reasons that were not observed with EPT; the exit reasons were 14 (INVLPG), 33 (VM-entry failure due to invalid guest state), and 58 (INVPCID).
