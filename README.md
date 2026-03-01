# MEIU 2022 Cross Platform Election Activity

## Table of Contents
- [Introduction](#introduction)
- [Justification of the Research](#justification-of-the-research)
- [Literature Review](#literature-review)
- [Research Questions](#research-questions)
- [Data](#data)
- [Notebooks](#notebooks)
- [Analysis 1: Cross-Platform Volume and Surge Detection](#analysis-1-cross-platform-volume-and-surge-detection)
- [Analysis 2: Measurement Comparison and Preliminary Harm Proxy](#analysis-2-measurement-comparison-and-preliminary-harm-proxy)
- [Conclusion](#conclusion)
- [Next Steps](#next-steps)
- [References](#references)

# Introduction / Justification of the Research

This project uses open, large-scale social media data to explore how election-related activity rises and falls across platforms during the 2022 U.S. midterm election season. The goal is to produce reproducible summaries and visual evidence that help form a clear prognostication: election-week attention surges cluster across platforms, but platforms are not perfectly synchronized (lead/lag patterns appear), and collection choices (candidate vs. keyword) materially change what “activity” looks like.

This research is important because cross-platform political activity is often discussed as if it were a single, unified signal, when in practice, platforms can behave differently in terms of volume and timing. By comparing daily activity patterns across platforms and identifying surge windows, this project helps clarify whether attention spikes are shared, staggered, or shaped by data-collection methods. In addition, by including a preliminary harm proxy and a supporting candidate-vs-keyword comparison, the project moves beyond simple counting and begins to test a more meaningful question: how platform dynamics and measurement choices affect what we interpret as election-related amplification?

# Literature Review

Scholarship on misinformation diffusion, online political attention, and cross-platform information dynamics informs this project. Research on misinformation and false news suggests that online attention can spread through diffusion and cascade-like processes, where relatively small bursts of activity can scale rapidly depending on platform affordances, user behavior, and network structure (Del Vicario et al., 2016; Beauvais, 2022). This framing is useful for the present project because it supports examining not only total volume but also the timing and intensity of attention spikes across platforms.

A second key insight in the literature is that platforms should not be treated as interchangeable environments. Prior research shows that platform design, visibility systems, and interaction patterns can produce distinct temporal signatures even when users respond to the same event (Avalle et al., 2024; Desiderio et al., 2025). This provides the rationale for a cross-platform approach: rather than presuming a single election “signal,” it compares daily surge behavior across platforms to assess whether activity peaks are synchronized, staggered, or shaped by platform-specific dynamics.

This project is also shaped by research on manipulation, automation, and the methodological limits of platform data. Studies on social bots and coordinated amplification show that political attention online can be strategically influenced and that the apparent scale of an issue may reflect both genuine engagement and engineered visibility (Desiderio et al., 2025). Related work on false-information ecosystems also highlights how narratives move across different online spaces and how platform-specific affordances can shape what becomes visible, repeated, or amplified (Adams et al., 2023).

This literature is especially relevant to my supporting comparison between candidate-based and keyword-based collection. A core methodological issue in this project is that “activity” is not a neutral object; it depends on how the dataset is assembled and what is counted. Comparing candidate vs. keyword collections in the same election-week window helps show how collection strategy can change the apparent magnitude of election-related attention. Accordingly, the measurement design should be understood as an integral component of the analysis itself rather than merely a technical preprocessing step.

The project’s preliminary harm proxy is informed by computational social science research on toxicity, offensive language, and hate speech detection. Prior work emphasizes that these categories are difficult to operationalize and that automated approaches can blur important distinctions between hateful content, abusive language, and more general offensive expression (Yin & Zubiaga, 2021; Wang et al., 2024). This is important for the present project because the harm indicator is intentionally designed as a lightweight, exploratory measure rather than a validated diagnostic instrument.

Recent empirical work from shared tasks and benchmark work on offensive language detection also shows both the usefulness and limitations of text classification approaches in social media settings (Ghosh et al., 2025). This literature frames the harm analysis as a first-pass proxy that can identify day-to-day variation and motivate follow-up testing, rather than as a basis for strong causal claims. Consequently, the project’s harm proxy is best understood as an exploratory signal that informs the development of a more precise next-stage research design.

# Research Questions
- **RQ1:** Which platform contributed the largest share of election-related items in MEIU22, and how does that distribution change over time?
- **RQ2:** What days show the strongest cross-platform surges, and are surges synchronized (same day across platforms) or staggered?
- **RQ3:** Do surge days correspond with increased hate/harassment signals (using a preliminary text-based harm proxy where available)?
- 

# Data Assembly

The primary dataset for this project comes from the MEIU22 repository, a multi-platform collection of election-related activity from the 2022 U.S. midterms. For this project, I assembled a working tidy index file (`meiu22_posts_index_tidy_labeled.csv`) that standardizes records across platforms and supports cross-platform comparison in a pandas workflow. The unit of analysis is one item (e.g., a post, tweet, or comment record), represented with fields such as `platform`, `collection_type`, `item_id`, `url`, `created_at_utc`, `relevant`, and `source_file`, along with a derived `day` field used for daily aggregation.

This project uses data from Facebook, Instagram, Reddit, and Twitter, with an important distinction: the Twitter keyword data in the tidy file is a 50k sample. In contrast, Twitter candidate data is available separately through dated candidate daily files in the repository. I selected this structure because it supports the project’s primary goals, daily trend comparisons, surge detection, and a supporting candidate-vs-keyword measurement comparison, while keeping the dataset in a format that is practical to clean, inspect, and analyze in pandas.

# Data Cleaning and Quality Checks

To prepare the data for time-based analysis, I created a usable `day` variable from available timestamps and, where needed, extracted dates from source filenames. This step is essential because the project’s main analyses (daily platform counts, z-score surge detection, and lead/lag interpretation) depend on consistent day-level comparability across platforms. I also checked for missing values and platform-specific gaps, especially in the `day` field, to identify where the dataset supports valid time-series comparisons and where it does not.

A major quality issue in the current tidy index is that the 50k Twitter keyword sample does not provide usable day-level timestamps, which limits Twitter’s comparability in the main cross-platform daily surge analysis. Rather than treating this as a coding failure, I treat it as a measurement constraint and adjust the analysis accordingly (focusing day-level surge detection on Facebook, Instagram, and Reddit while using Twitter candidate daily files to improve coverage). These checks also help define what is included, excluded, or interpreted cautiously in later analyses, which is an important part of making the project reproducible and analytically transparent.

