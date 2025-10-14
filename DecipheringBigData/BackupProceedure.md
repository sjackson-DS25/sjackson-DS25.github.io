
#  Unit 11 - Backup Proceedure

---

## Task

In 100 - 150 words, critically evaluate the disaster recovery system called the “Grandfather-Father-Son" GFS backup procedure. highlight how the strategy makes the back-up of large databases less resource heavy and argue whether this strategy is effective when compared to other methods of backing up data.  


## Critical Evaluation of the Grandfather-Father-Son (GFS) Backup Strategy

The Grandfather-Father-Son (GFS) system provides a tiered, storage efficient backup system by undertaking Son (daily), Father (weekly) and Grandfather (monthly) backups on a rotating schedule. The benefits of this method include comprehensive data protection, rapid recovery of recent data, long-term historical data preservation, regulatory compliance and risk mitigation (Nheu, 2024).

This tiered structure balances quick data recovery with long-term archival storage, and the rotational backup strategy optimises storage management by allowing Grandfather backups to be retained within cost efficient storage solutions (Casas, 2025) whilst Son and Father backups are retained for comparatively shorter periods (Casas, 2025) 

The GFS system reduces complications associated with incremental backups by eliminating the need to restore from a full backup plus every incremental backup to reach the current state.  It additionally mitigates the risk of a corrupted backup being propagated throughout an extended backup chain.  Compared to differential backups methods, the GFS approach optimises storage requirements. (Gibraltar Solutions, 2024) 

## References

Casas, M. (2025) Grandfather-Father-Son Backup Strategy: Storage Optimisation and Retention Management. Trilio. Available at: https://trilio.io/resources/grandfather-father-son-backup/ (Accessed: 12 October 2025).

Gibraltar Solutions. (2024) Mastering the Art of Modern Data Backup Strategies. Gibraltar Solutions. Available at: https://gibraltarsolutions.com/blog/mastering-the-art-of-modern-data-backup-strategies/ (Accessed: 12 October 2025).

Nheu, W. (2024) ‘The Grandfather-Father-Son Backup Scheme Explained’, BackupAssist, 9 April. Available at: https://www.backupassist.com/blog/the-grandfather-father-son-backup-scheme-explained (Accessed: 12 October 2025).

<hr>

<a href="https://sjackson-ds25.github.io/DecipheringBigData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Deciphering Big Data</a>

<hr>
