Тут буду пробувавти поняти що за бакети хот варм колд фроузен і тп.

Все роблю на кластері.


Створення індекса буде виконуватись як описано [тут](https://github.com/0x0xxix0/Splunk/blob/main/How-To/add-index-in-cluster.md) 


На МС створюється апка в `$SPLUNK_HOME/etc/manager-apps` після пушу на І вона потрапляє в `/opt/splunk/etc/pears_app/<index_name>`


Індекс описується файлі `indexes.conf`

Кожен індекс за замовчуванням повинен містити шляхи до `db` `colddb` `thaweddb` 

За замовчуванням та bestPractic всі db colddb повинні бути в `$SPLUNK_DB/<index_name>/db` та `$SPLUNK_DB/<index_name>/colddb` відповідно.



В такому випадку в $SPLUNK_dB/<index_name> буде таке дерево файлів.

```
/<index_name>
├── datamodel_summary <------------------------------ Тут будуть міститись якісь скорочені шляхи при використанні даного індекса в Дата моделі
├── thaweddb <--------------------------------------- Сюди потраплять розморожені дані з індекса коли виконуватиметься процес frozen --> thawed 
├── colddb <----------------------------------------- Сюди переносяться warm buckets при досяганні певного часу старості, або при досяганні максимальної к-ті warmбакетів
└── db 	< ------------------------------------------- Тут зберігаються hot та warm buckets 
    ├── CreationTime <------------------------------- Час створення перешого bucket
    ├── GlobalMetaData <----------------------------- Дирикторія де буду зберігатись якась глобальна мета дата
    ├── db_<first_time>_<last_time>_<bucket_id> <---- Warm bucket в назві відображається час першого та останнього event-у який міститься в даному бакеті 
    │   ├── Strings.data <--------------------------- Тут зберігаються якісь мітки часу
    │   ├── <first_time>-<last_time>-<id>.tsidx <---- В таких файлах зберігаються посилання кожного key в event до його місця в rawdata
    │   ├── Hosts.data <----------------------------- Перелік host які містяться в даному бакеті
    │   ├── Sources.data <--------------------------- Перелік source які містяться в даному бакеті
    │   ├── SourceTypes.data <----------------------- Перелік sourcetype які містяться в даному бакеті
    │   ├── bloomfilter <---------------------------- ??? дізнаюсь напишу тут якісь блум фільтри які також можна для чогось юзати 
    │   ├── bucket_info.csv <------------------------ Час індескації першого та останнього івенту
    │   ├── optimize.result <------------------------  ??
    │   └── rawdata. <------------------------------- Тут зберігаються _raw data
    │       ├── journal.zst <------------------------ ?? Так низько я не опускався
    │       └── slicesv2.dat <----------------------- ?? Так низько я не опускався
    │       
    └── hot_v1_8 <----------------------------------- Тут зберігаються hot bucket 
        ├── bucket_info.csv <------------------------ Час індескації першого та останнього івенту
        ├── Strings.data <--------------------------- Тут зберігаються якісь мітки часу
        ├── <first_time>-<last_time>-<id>.tsidx <---- В таких файлах зберігаються посилання кожного key в event до його місця в rawdata
        └── rawdata <-------------------------------- Тут зберігаються _raw data
            ├── journal.zst <------------------------ ?? Так низько я не опускався
            └── slicesv2.dat <----------------------- ?? Так низько я не опускався
             

```

```
[volume:<volume_for_hot>]
path = /mnt/fast_disk
maxVolumeDataSizeMB = 200

+----------------- 200 MB ------+ <--- maxVolumeDataSizeMB
| ~/                            |	
| |_ /mnt                       |
|   |                           |
|   |_ fast_disk <--------------|----------- path 
|                               |
+-------------------------------+

[volume:<volume_for_cold>]
path = /mnt/big_disk
maxVolumeDataSizeMB = 900

+---------------- 900 MB -------+ <--- maxVolumeDataSizeMB
| ~/                            |	
| |_ /mnt                       |
|   |                           |
|   |_ big_disk <---------------|----------- path 
|                               |
+-------------------------------+


```


Bucket ROADMAP
```
HOT <--  Записуються дані
 |
 | <-- Коли HOT досягає певного розміру maxDataSize / Індексер перезавантажуєтся
 |
WARM
 |
 | <-- Досягається максимальна кількість допустимих WARM бакетів maxWarmDBCount
 |
COLD
 |
 | <-- COLD бакет досягає віку frozenTimePeriodInSecs / перевищується ліміт памяті в coldPath.maxDataSizeMB 
 |
FROZEN
 |
 | <-- Відбувається розмороження індекса ХЗ поки як це але типа класна
 |
Thawed
```



## Важливі атрибути в `indexes.conf` для самого `[<index_name>]`

### `maxTotalDataSizeMB` - Максимальний розмір індекса в мегабайтах 

* Якщо індес виросте до цього розміру splunk перемістить найстарші дані в стан frozen або видалить.

* Даний атрибут впливає лише на hot warm та cold 

* CAUTION: The 'maxTotalDataSizeMB' size limit can be reached before the time 
  limit defined in 'frozenTimePeriodInSecs' due to the way bucket time spans 
  are calculated. When the 'maxTotalDataSizeMB' limit is reached, the buckets 
  are rolled to frozen. As the default policy for frozen data is deletion, 
  unintended data loss could occur.

* Splunkd нехтує цим атрибутом якщо індекс на віддаленому зберіганні. 

* Highest legal value is 4294967295 

* Default: 500000



### `datatype = <event|metric>` - тип індекса Для нас це event

* Determines whether the index stores log events or metric data.
* If set to "metric", the indexer optimizes the index to store metric
  data which can be  queried later only using the 'mstats' operator,
  as searching metric data is different from traditional log events.
* Use the "metric" data type only for metric sourcetypes like statsd.
* Optional.
* Default: event



## Важливі атрибути в `indexes.conf` для HOT та WARM
	
###  `homePath` - директорія куди записуватимуться hot та warm
	
Оскільки Warm відрізняється від Hot тільки тим, що в нього безпосередньо відбувається запис даних які надходять, тому вони знаходяться в одному місці
(__Швидкий диск__) шлях до якого визначає даний атрибут.

`
[<index_name>]
homePath = $SPLUNK_DB/<index_name>/db`

> Примітка: Якщо не використовується __volume__.

Volume створює посилання на місце в тачці та описує його.

Приклад стоврення та використання volume.

```
[volume:hot1]
path = /mnt/fast_disk
maxVolumeDataSizeMB = 100000

[idx1]
homePath = volume:hot1/idx1
```


### `maxDataSize = <positive integer>|auto|auto_high_volume` - максимальний розмір hot bucketa

* The maximum size, in megabytes, that a hot bucket can reach before splunkd
  triggers a roll to warm.
* Specifying "auto" or "auto_high_volume" will cause Splunk to autotune this
  setting (recommended).
* You should use "auto_high_volume" for high-volume indexes (such as the
  main index); otherwise, use "auto". A "high volume index" would typically
  be considered one that gets over 10GB of data per day.
* "auto_high_volume" sets the size to 10GB on 64-bit, and 1GB on 32-bit
  systems.
* Although the maximum value you can set this is 1048576 MB, which
  corresponds to 1 TB, a reasonable number ranges anywhere from 100 to
  50000. Before proceeding with any higher value, please seek approval of
  Splunk Support.
* If you specify an invalid number or string, maxDataSize will be auto
  tuned.
* NOTE: The maximum size of your warm buckets might slightly exceed
  'maxDataSize', due to post-processing and timing issues with the rolling
  policy.
* For remote storage enabled indexes, consider setting this value to "auto"
  (750MB) or lower.
* Default: "auto" (sets the size to 750 megabytes)


### `maxHotBuckets = <positive integer> | auto` - максимальна кількість hot бакетів які можуть бути в нідексі

* Maximum number of hot buckets that can exist per index.
* When 'maxHotBuckets' is exceeded, the indexer rolls the hot bucket
  containing the least recent data to warm.
* Both normal hot buckets and quarantined hot buckets count towards this
  total.
* This setting operates independently of maxHotIdleSecs, which can also
  cause hot buckets to roll.
* NOTE: the indexer applies this limit per ingestion pipeline. For more
  information about multiple ingestion pipelines, see
  'parallelIngestionPipelines' in the server.conf.spec file.
* With N parallel ingestion pipelines, the maximum number of hot buckets across
  all of the ingestion pipelines is N * 'maxHotBuckets', but only
  'maxHotBuckets' for each ingestion pipeline. Each ingestion pipeline
  independently writes to and manages up to 'maxHotBuckets' number of hot
  buckets. Consequently, when multiple ingestion pipelines are configured, there
  may be multiple hot buckets with events on overlapping time ranges.
* The highest legal value is 1024. However, do not set to a value greater 
  than 11 without direction from Splunk Support. Higher values can degrade 
  indexing performance.
* If you specify "auto", the indexer sets the value to 3.
* This setting applies only to event indexes.
* Default: "auto"



### `maxWarmDBCount = <nonnegative integer>` - максимальна кількість warm бакетів яка може бути в індексі 
* The maximum number of warm buckets.
* Warm buckets are located in the 'homePath' for the index.
* If set to zero, splunkd does not retain any warm buckets
  It rolls the buckets to cold as soon as it is able.
* Splunkd ignores this setting on remote storage enabled indexes.
* Highest legal value is 4294967295.
* Default: 300




### ` warmToColdScript= <script path>` - 
 
* Specifies a script to run when moving data from warm to cold buckets.
* This setting is supported for backwards compatibility with versions
  older than 4.0. Migrating data across filesystems is now handled natively
  by splunkd.
* If you specify a script here, the script becomes responsible for moving
  the event data, and Splunk-native data migration is not used.
* The script must accept two arguments:
  * First: the warm directory (bucket) to be rolled to cold.
  * Second: the destination in the cold path.
* If the script you specify is a Python script, it uses the default system-wide
  Python interpreter. You cannot override this configuration with the               
  'python.version' setting.
* Searches and other activities are paused while the script is running.
* Contact Splunk Support (http://www.splunk.com/page/submit_issue) if you
  need help configuring this setting.
* The script must be in $SPLUNK_HOME/bin or a subdirectory thereof.     
* Splunkd ignores this setting for remote storage enabled indexes.
* Default: empty string
                                                                                                                                                                      

## Важливі атрибути в `indexes.conf` для cold

### `coldPath` - де будуть розміщуватись cold бакети


### `coldToFrozenScript ` -  ??? хз якщо без нього
coldToFrozenScript = <path to script interpreter> <path to script>
* Specifies a script to run when data is to leave the splunk index system.
  * Essentially, this implements any archival tasks before the data is
    deleted out of its default location.
* Add "$DIR" (including quotes) to this setting on Windows (see below
  for details).
* Script Requirements:
  * The script must accept at least one argument: An absolute path to the bucket directory
    that is to be archived.
  * In the case of metrics indexes, the script must also accept the flag "--search-files-required",
    to prevent the script from archiving empty rawdata files. For more details, see the entry for the
    "metric.stubOutRawdataJournal" setting.
  * Your script should work reliably.
    * If your script returns success (0), Splunk completes deleting
      the directory from the managed index location.
    * If your script return failure (non-zero), Splunk leaves the bucket
      in the index, and tries calling your script again several minutes later.
    * If your script continues to return failure, this will eventually cause
      the index to grow to maximum configured size, or fill the disk.
  * Your script should complete in a reasonable amount of time.
    * If the script stalls indefinitely, it will occupy slots.
    * This script should not run for long as it would occupy
      resources which will affect indexing.
* If the string $DIR is present in this setting, it will be expanded to the
  absolute path to the directory.
* If $DIR is not present, the directory will be added to the end of the
  invocation line of the script.
  * This is important for Windows.
    * For historical reasons, the entire string is broken up by
      shell-pattern expansion rules.
    * Since Windows paths frequently include spaces, and the Windows shell
      breaks on space, the quotes are needed for the script to understand
      the directory.
* If your script can be run directly on your platform, you can specify just
  the script.
  * Examples of this are:
    * .bat and .cmd files on Windows
    * scripts set executable on UNIX with a #! shebang line pointing to a
      valid interpreter.
* You can also specify an explicit path to an interpreter and the script.
    * Example:  /path/to/my/installation/of/python.exe path/to/my/script.py
* Splunk software ships with an example archiving script in that you SHOULD
  NOT USE $SPLUNK_HOME/bin called coldToFrozenExample.py
  * DO NOT USE the example for production use, because:
    * 1 - It will be overwritten on upgrade.
    * 2 - You should be implementing whatever requirements you need in a
          script of your creation. If you have no such requirements, use
          'coldToFrozenDir'
* Example configuration:
  * If you create a script in bin/ called our_archival_script.py, you could use:
    UNIX:
        coldToFrozenScript = "$SPLUNK_HOME/bin/python" \
          "$SPLUNK_HOME/bin/our_archival_script.py"
    Windows:
        coldToFrozenScript = "$SPLUNK_HOME/bin/python" \
          "$SPLUNK_HOME/bin/our_archival_script.py" "$DIR"
* The example script handles data created by different versions of Splunk
  differently. Specifically, data from before version 4.2 and after version 4.2
  are handled differently. See "Freezing and Thawing" below:
* The script must be in $SPLUNK_HOME/bin or a subdirectory thereof.
* No default.


### `frozenTimePeriodInSecs ` - кількість секунд після яких дані індекса підуть в frozen

якщо ВСІ івенти старші за цей час. то вони в frozen падають.
 = <nonnegative integer>
* The number of seconds after which indexed data rolls to frozen.
* If you do not specify a 'coldToFrozenScript', data is deleted when rolled to
  frozen.
* NOTE: Every event in a bucket must be older than 'frozenTimePeriodInSecs'
  seconds before the bucket rolls to frozen.
* The highest legal value is 4294967295.
* Default: 188697600 (6 years)




### `coldPath.maxDataSizeMB = <nonnegative integer>` - максимальний розмір якого може досягнути coldPath після переходить в frozen
* Specifies the maximum size of 'coldPath' (which contains cold buckets).
* If this size is exceeded, splunkd freezes buckets with the oldest value
  of latest time (for a given bucket) until coldPath is below the maximum
  size.
* If you set this setting to 0, or do not set it, splunkd does not constrain the
  size of 'coldPath'.
* If splunkd freezes buckets due to enforcement of this setting, and
  'coldToFrozenScript' and/or 'coldToFrozenDir' archiving settings are also
  set on the index, these settings are used.
* Splunkd ignores this setting for remote storage enabled indexes.
* The highest legal value is 4294967295.
* Default: 0


## Важливі атрибути в `indexes.conf` для frozen



### `coldToFrozenDir`  - Де будуть розсташовані frozen

coldToFrozenDir = <path to frozen archive>
* An alternative to a 'coldToFrozen' script - this setting lets you
  specify a destination path for the frozen archive.
* Splunk software automatically puts frozen buckets in this directory
* For information on how buckets created by different versions are
  handled, see "Freezing and Thawing" below.
* If both 'coldToFrozenDir' and 'coldToFrozenScript' are specified,
  'coldToFrozenDir' takes precedence
* You must restart splunkd after changing this setting. Reloading the
  configuration does not suffice.
* May NOT contain a volume reference.
	
	
## Важливі атрибути в `indexes.conf` для thawed // поки в це не вникати дуже

### `thawedPath` - куди будуть потрапляти дані при їх переходу з frozen в thawed 

	

## Важливі атрибути в `indexes.conf` для datamodel_summary


### `tstatsHomePath` - Де буде розміщуватись datamodel_summary

* Location where data model acceleration TSIDX data for this index should be stored.
* Required.
* MUST be defined in terms of a volume definition (see volume section below)
* Path must be writable.
* You must not set this setting for remote storage enabled indexes.
* You must restart splunkd after changing this setting for the
  changes to take effect. Reloading the index configuration does
  not suffice.
* Default: volume:_splunk_summaries/$_index_name/datamodel_summary,
  where "$_index_name" is runtime-expanded to the name of the index


### `summaryHomePath` - шлях де повиннні зберігатись якісь штуки повязані з ДМ

* An absolute path where transparent summarization results for data in this
  index should be stored.
* This value must be different for each index and can be on any disk drive.
* Best practice is to specify the path with the following syntax:
     summaryHomePath = $SPLUNK_DB/$_index_name/summary
  At runtime, splunkd expands "$_index_name" to the name of the index.
  For example, if the index name is "newindex", summaryHomePath becomes
  "$SPLUNK_DB/newindex/summary".
* Can contain a volume reference (see volume section below) in place of $SPLUNK_DB.
* Volume reference must be used if you want to retain data based on data size.
* Path must be writable.
* If not specified, splunkd creates a directory 'summary' in the same
  location as 'homePath'.
  * For example, if 'homePath' is "/opt/splunk/var/lib/splunk/index1/db",
    then 'summaryHomePath' must be "/opt/splunk/var/lib/splunk/index1/summary".
* The parent path must be writable.
* You must not set this setting for remote storage enabled indexes.
* You must restart splunkd after changing this setting for the
  changes to take effect. Reloading the index configuration does
  not suffice.
* Avoid the use of environment variables in index paths, aside from the
  exception of SPLUNK_DB. See 'homePath' for additional
  information as to why.
* No default.





