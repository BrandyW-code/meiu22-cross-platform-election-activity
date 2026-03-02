# MEIU 2022 Cross Platform Election Activity

## Table of Contents
- [Introduction / Justification of the Research](#introduction--justification-of-the-research)
- [Literature Review](#literature-review)
- [Research Questions](#research-questions)
- [Data Assembly](#data-assembly)
- [Data Cleaning and Quality Checks](#data-cleaning-and-quality-checks)
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

A major quality issue in the current tidy index is that the 50k Twitter keyword sample does not provide usable day-level timestamps, which limits Twitter’s comparability in the main cross-platform daily surge analysis. Rather than treating this as a coding failure, I treat it as a measurement constraint and adjust the analysis accordingly (focusing day-level surge detection on Facebook, Instagram, and Reddit while using Twitter candidate daily files to improve coverage). These checks also help define what is included, excluded, or interpreted cautiously in later analysis, which is an important part of making the project reproducible and analytically transparent.

## Notebooks

Note: The notebooks include step-by-step documentation of the data assembly, cleaning, and analysis workflow used to generate the tables and figures in this project.

- [Analysis 1: Cross-Platform Volume and Surge Detection (separate repo)](notebooks/analysis_1_cross_platform_volume_and_surge_detection.ipynb)](https://github.com/BrandyW-code/Analysis_1_cross_platform_volume_and_surge_detection.ipynb/blob/main/README.md)
- [Analysis 2: Supporting Measurement Analysis and Preliminary Harm Proxy](notebooks/analysis_2_supporting_measurement_and_harm_proxy.ipynb)

# Conclusion

This project used the MEIU22 collection to assemble a cross-platform, daily-level view of election-related activity and test an initial suspicion about synchronization, lead/lag timing, and measurement effects. Using a tidy item-level index, I aggregated daily platform counts, applied z-score surge detection, and compared top surge days across Facebook, Instagram, and Reddit. I also added a supporting measurement analysis (candidate vs. keyword collection in the Nov 4–10 window) and a preliminary Reddit-based harm proxy to explore whether surge periods may coincide with shifts in harm-related signals.

The results suggest that election-week attention surges cluster in a shared window, especially around Nov 7–9, but are not perfectly synchronized: Facebook and Instagram peak on 11-08-2022, while Reddit peaks on 11-09-2022. This supports a lead/lag interpretation rather than a fully simultaneous cross-platform spike. The supporting candidate-vs-keyword comparison also shows that collection strategy materially changes what “activity” looks like, reinforcing that cross-platform comparisons are partly measurement comparisons. Together, these findings indicate that the project’s suspicion is strong enough to justify additional data collection and more rigorous testing.

The project remains exploratory and has clear limitations, particularly the Twitter keyword sample’s limited day-level comparability and the lightweight harm proxy. These constraints do not invalidate the findings, but they do shape what can be concluded at this stage. The value of this pilot is that it has produced a reproducible workflow, identified meaningful surge patterns, and generated a clear path for next-stage analysis focused on stronger temporal comparability, formal lead/lag testing, and improved harm measurement.

# Next Steps

Next steps focus on strengthening each research question by using more comparable measurements across platforms. For RQ1 (cross-platform volume), I will extend the daily coverage and ensure platform counts reflect comparable time windows by rebuilding the Twitter timeline using the candidate daily files (which embed dates) and, where permitted, adding timestamp sources so Twitter can be included in the same day-level distribution plots as Facebook/Instagram/Reddit. For RQ2 (surge detection), I will move beyond top-day lists by formally testing synchronization and lead/lag structure using cross-correlation and lagged comparisons across platform time series (e.g., whether Facebook/Instagram peaks systematically precede Reddit peaks by about 1 day during major events).

For RQ3 (harm proxy/escalation), I will replace the current lightweight text indicator with a stronger harm measure, either a validated toxicity/hate detection model or a secondary harm-proxy dataset aligned by date, and then test whether surge days (high z-score days) are associated with statistically higher harm rates than baseline days. I also plan to add narrative context on surge days using text-based exploration where available (for example, Reddit KWIC and clustering; Twitter only via permitted hydration), so the project can move from descriptive surge patterns toward a stronger, evidence-backed account of platform timing and potential escalation signals. Longer term, this workflow could support ongoing monitoring, larger-scale collection, and more advanced modeling as the dataset and measures improve.

# References

Adams, Z., Osman, M., Bechlivanidis, C., & Meder, B. (2023). (Why) is misinformation a problem? Perspectives on Psychological Science, 18(6), 1436–1463. https://doi.org/10.1177/17456916221141344

Avalle, M., Di Marco, N., Etta, G., Sangiorgio, E., Alipour, S., Bonetti, A., Alvisi, L., Scala, A., Baronchelli, A., Cinelli, M., & Quattrociocchi, W. (2024). Persistent interaction patterns across social media platforms and over time. Nature, 628(8008), 582–589. https://doi.org/10.1038/s41586-024-07229-y

Beauvais, C. (2022). Fake news: Why do we believe it? Joint Bone Spine, 89(4), 105371. https://doi.org/10.1016/j.jbspin.2022.105371

Del Vicario, M., Bessi, A., Zollo, F., Petroni, F., Scala, A., Caldarelli, G., Stanley, H. E., & Quattrociocchi, W. (2016). The spreading of misinformation online. Proceedings of the National Academy of Sciences, 113(3), 554–559. https://doi.org/10.1073/pnas.1517441113

Desiderio, A., Mancini, A., Cimini, G., & Di Clemente, R. (2025). Highly engaging events reveal semantic and temporal compression in online community discourse. PNAS Nexus, 4(3), pgaf056. https://doi.org/10.1093/pnasnexus/pgaf056

Ghosh, K., Saha, S., Mandl, T., & Modha, S. (2025). Findings from shared tasks on hate speech detection: Performance patterns for low-resource languages. Pattern Recognition Letters, 199, 303–309. https://doi.org/10.1016/j.patrec.2025.09.004

Wang, X., Koneru, S., Venkit, P. N., Frischmann, B., & Rajtmajer, S. (2024). The unappreciated role of intent in algorithmic moderation of social media content. arXiv (Cornell University). https://doi.org/10.48550/arxiv.2405.11030

Yin, W., & Zubiaga, A. (2021). Towards generalisable hate speech detection: a review on obstacles and solutions. PeerJ Computer Science, 7, e598. https://doi.org/10.7717/peerj-cs.598

