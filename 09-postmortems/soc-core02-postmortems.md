## 📌 Post-Mortem: soc-core02 (Wazuh Overwrite)

During Iron Watch 02, the SOC VM `soc-core02` was intentionally decommissioned
after a failed overwrite of the Wazuh stack (manager, indexer, dashboard).

Rather than continuing with an untrustworthy detection platform, the decision
was made to rebuild the SOC from a clean baseline (`soc-core03`).

A detailed post-mortem is available here:
➡️ [When a SIEM Reinstall Goes Wrong: A Wazuh Post-Mortem](BLOG_LINK_HERE)

This incident improved the overall architecture, documentation quality,
and operational reliability of Iron Watch 02.
