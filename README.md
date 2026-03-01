# MEIU 2022 Cross Platform Election Activity
# Project Overview 
This Data Wrangling final project examines cross-platform election-related activity during the 2022 U.S. midterm period using the MEIU22 dataset. The project focuses on platform volume over time, surge detection (including shared surge windows and possible lead/lag patterns), and an exploratory harm proxy using Reddit text.   

**Research Questions**
- **RQ1:** Which platform contributed the largest share of election-related items in MEIU22, and how does that distribution change over time?
- **RQ2:** What days show the strongest cross-platform surges, and are surges synchronized (same day across platforms) or staggered?
- **RQ3:** Do surge days correspond with increased hate/harassment signals (using a preliminary text-based harm proxy where available)?

# Introduction / Justification of the Research

This project uses open, large-scale social media data to explore how election-related activity rises and falls across platforms during the 2022 U.S. midterm election season. The goal is to produce reproducible summaries and visual evidence that help form a clear prognostication: election-week attention surges cluster across platforms, but platforms are not perfectly synchronized (lead/lag patterns appear), and collection choices (candidate vs. keyword) materially change what “activity” looks like.

This research is important because cross-platform political activity is often discussed as if it were a single, unified signal, when in practice, platforms can behave differently in terms of volume and timing. By comparing daily activity patterns across platforms and identifying surge windows, this project helps clarify whether attention spikes are shared, staggered, or shaped by data-collection methods. In addition, by including a preliminary harm proxy and a supporting candidate-vs-keyword comparison, the project moves beyond simple counting and begins to test a more meaningful question: how platform dynamics and measurement choices affect what we interpret as election-related amplification?

