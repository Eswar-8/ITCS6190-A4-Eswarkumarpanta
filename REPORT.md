# Assignment #4: Report

**Name:** Eswar Kumar Panta

**Student ID:** 801505751

**Email:** epanta@charlotte.edu

---

## Seed and commands

Seed used for `datagen.py`: 801505751 

The commands you ran, in order:

```bash
python3 datagen.py 801505751

docker compose -f docker-compose.codespaces.yml up -d      

docker cp main.py spark-master:/opt/spark/work-dir/
docker exec -it spark-master /opt/spark/bin/spark-submit --master spark://spark-master:7077 \
    /opt/spark/work-dir/main.py part1a /opt/spark/work-dir/shared 801505751
docker cp main.py spark-master:/opt/spark/work-dir/
docker exec -it spark-master /opt/spark/bin/spark-submit --master spark://spark-master:7077 \
    /opt/spark/work-dir/main.py part1b /opt/spark/work-dir/shared 801505751
docker cp main.py spark-master:/opt/spark/work-dir/
docker exec -it spark-master /opt/spark/bin/spark-submit --master spark://spark-master:7077 \
    /opt/spark/work-dir/main.py part2 /opt/spark/work-dir/shared 801505751
docker cp main.py spark-master:/opt/spark/work-dir/
docker exec -it spark-master /opt/spark/bin/spark-submit --master spark://spark-master:7077 \
    /opt/spark/work-dir/main.py part3 /opt/spark/work-dir/shared 801505751
python3 datagen.py 801505751 shared-folder /tmp/truth.csv
```

---

## Part 1a: trips and revenue per zone per window

The micro-batch summary (batches, watermark, state rows):

```
 === part1a: micro-batch summary ===
batch input rows            watermark state rows
    5         35  2026-10-27 08:01:47         39
    6         45  2026-10-27 08:02:54         49
    7         41  2026-10-27 08:03:57         50
    8         43  2026-10-27 08:04:50         50
    9         37  2026-10-27 08:05:54         25
   10         44  2026-10-27 08:06:49         41
   11         40  2026-10-27 08:07:49         47
   12         38  2026-10-27 08:08:51         49
   13         37  2026-10-27 08:09:46         49
   14         39  2026-10-27 08:10:53         25
   15         39  2026-10-27 08:11:46         38
   16         35  2026-10-27 08:12:44         46
   17         36  2026-10-27 08:13:58         46
   18         44  2026-10-27 08:14:49         46
   19         45  2026-10-27 08:15:56         24
   20         45  2026-10-27 08:16:47         39
   21         35  2026-10-27 08:17:48         45
   22         40  2026-10-27 08:18:50         47
   23         38  2026-10-27 08:19:57         49
   24         39  2026-10-27 08:20:51         25
   25         43  2026-10-27 08:21:48         42
   26         36  2026-10-27 08:22:51         49
   27         38  2026-10-27 08:23:48         50
   28         43  2026-10-27 08:24:49         50
   29         39  2026-10-27 08:25:46         25
   30          0  2026-10-27 08:26:43         25

=== part1a: windows written by the stream ===
+-------------------+-------------------+-----------+-----+-------+
|window_start       |window_end         |pickup_zone|trips|revenue|
+-------------------+-------------------+-----------+-----+-------+
|2026-10-27 07:55:00|2026-10-27 08:00:00|1          |1    |17.78  |
|2026-10-27 07:55:00|2026-10-27 08:00:00|3          |1    |22.9   |
|2026-10-27 07:55:00|2026-10-27 08:00:00|5          |3    |78.72  |
|2026-10-27 07:55:00|2026-10-27 08:00:00|12         |1    |21.49  |
|2026-10-27 07:55:00|2026-10-27 08:00:00|13         |1    |41.21  |
|2026-10-27 07:55:00|2026-10-27 08:00:00|15         |1    |28.67  |
|2026-10-27 07:55:00|2026-10-27 08:00:00|19         |1    |22.39  |
|2026-10-27 07:55:00|2026-10-27 08:00:00|23         |1    |3.71   |
|2026-10-27 08:00:00|2026-10-27 08:05:00|1          |23   |622.23 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|2          |10   |226.42 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|3          |20   |319.87 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|4          |5    |123.99 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|5          |19   |566.54 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|6          |5    |85.03  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|7          |5    |132.38 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|8          |8    |168.27 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|9          |3    |54.62  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|10         |8    |148.42 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|11         |15   |364.38 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|12         |2    |41.66  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|13         |5    |88.05  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|14         |14   |262.3  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|15         |1    |28.59  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|16         |5    |158.02 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|17         |8    |165.47 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|18         |4    |55.37  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|19         |10   |273.04 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|20         |12   |317.99 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|21         |3    |69.93  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|22         |3    |64.4   |
|2026-10-27 08:00:00|2026-10-27 08:05:00|23         |11   |322.88 |
|2026-10-27 08:00:00|2026-10-27 08:05:00|24         |3    |79.88  |
|2026-10-27 08:00:00|2026-10-27 08:05:00|25         |8    |150.15 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|1          |15   |344.9  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|2          |8    |137.05 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|3          |25   |489.24 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|4          |8    |129.43 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|5          |25   |797.3  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|6          |2    |37.08  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|7          |4    |89.72  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|8          |7    |162.22 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|9          |2    |23.64  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|10         |7    |108.24 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|11         |17   |345.66 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|12         |1    |8.18   |
|2026-10-27 08:05:00|2026-10-27 08:10:00|13         |9    |180.76 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|14         |6    |128.3  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|15         |1    |20.22  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|16         |6    |116.98 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|17         |11   |242.09 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|18         |2    |30.99  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|19         |9    |172.4  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|20         |8    |187.05 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|21         |3    |60.07  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|22         |5    |133.7  |
|2026-10-27 08:05:00|2026-10-27 08:10:00|23         |7    |112.45 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|24         |6    |173.02 |
|2026-10-27 08:05:00|2026-10-27 08:10:00|25         |8    |203.73 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|1          |16   |419.76 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|2          |9    |187.56 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|3          |20   |493.27 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|4          |3    |54.8   |
|2026-10-27 08:10:00|2026-10-27 08:15:00|5          |16   |402.58 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|6          |4    |132.94 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|7          |6    |137.45 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|8          |10   |224.55 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|9          |3    |68.88  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|10         |7    |170.5  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|11         |15   |258.12 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|12         |4    |80.47  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|13         |9    |282.11 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|14         |7    |136.48 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|15         |6    |136.15 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|16         |4    |91.58  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|17         |8    |207.18 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|18         |6    |164.03 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|19         |6    |93.07  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|20         |9    |151.73 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|21         |5    |182.79 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|22         |3    |100.83 |
|2026-10-27 08:10:00|2026-10-27 08:15:00|23         |13   |298.0  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|24         |2    |15.69  |
|2026-10-27 08:10:00|2026-10-27 08:15:00|25         |6    |114.3  |
|2026-10-27 08:15:00|2026-10-27 08:20:00|1          |17   |435.59 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|2          |7    |98.03  |
|2026-10-27 08:15:00|2026-10-27 08:20:00|3          |19   |352.01 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|4          |6    |79.94  |
|2026-10-27 08:15:00|2026-10-27 08:20:00|5          |22   |618.73 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|6          |6    |132.46 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|7          |9    |197.79 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|8          |12   |249.17 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|9          |2    |27.25  |
|2026-10-27 08:15:00|2026-10-27 08:20:00|10         |8    |126.58 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|11         |14   |300.89 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|13         |8    |124.17 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|14         |12   |258.01 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|15         |1    |14.87  |
|2026-10-27 08:15:00|2026-10-27 08:20:00|16         |7    |133.52 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|17         |8    |151.48 |
|2026-10-27 08:15:00|2026-10-27 08:20:00|18         |4    |59.77  |
+-------------------+-------------------+-----------+-----+-------+
only showing top 100 rows
=== part1a: the same aggregation in batch over all files ===
+-------------------+-----------+-----+-------+
|window_start       |pickup_zone|trips|revenue|
+-------------------+-----------+-----+-------+
|2026-10-27 07:55:00|1          |1    |17.78  |
|2026-10-27 07:55:00|3          |1    |22.9   |
|2026-10-27 07:55:00|5          |3    |78.72  |
|2026-10-27 07:55:00|12         |1    |21.49  |
|2026-10-27 07:55:00|13         |1    |41.21  |
|2026-10-27 07:55:00|15         |1    |28.67  |
|2026-10-27 07:55:00|19         |1    |22.39  |
|2026-10-27 07:55:00|23         |1    |3.71   |
|2026-10-27 08:00:00|1          |23   |622.23 |
|2026-10-27 08:00:00|2          |10   |226.42 |
|2026-10-27 08:00:00|3          |20   |319.87 |
|2026-10-27 08:00:00|4          |5    |123.99 |
|2026-10-27 08:00:00|5          |19   |566.54 |
|2026-10-27 08:00:00|6          |5    |85.03  |
|2026-10-27 08:00:00|7          |5    |132.38 |
|2026-10-27 08:00:00|8          |8    |168.27 |
|2026-10-27 08:00:00|9          |3    |54.62  |
|2026-10-27 08:00:00|10         |8    |148.42 |
|2026-10-27 08:00:00|11         |15   |364.38 |
|2026-10-27 08:00:00|12         |2    |41.66  |
|2026-10-27 08:00:00|13         |6    |107.67 |
|2026-10-27 08:00:00|14         |14   |262.3  |
|2026-10-27 08:00:00|15         |1    |28.59  |
|2026-10-27 08:00:00|16         |5    |158.02 |
|2026-10-27 08:00:00|17         |8    |165.47 |
|2026-10-27 08:00:00|18         |4    |55.37  |
|2026-10-27 08:00:00|19         |10   |273.04 |
|2026-10-27 08:00:00|20         |12   |317.99 |
|2026-10-27 08:00:00|21         |3    |69.93  |
|2026-10-27 08:00:00|22         |3    |64.4   |
|2026-10-27 08:00:00|23         |11   |322.88 |
|2026-10-27 08:00:00|24         |3    |79.88  |
|2026-10-27 08:00:00|25         |8    |150.15 |
|2026-10-27 08:05:00|1          |15   |344.9  |
|2026-10-27 08:05:00|2          |8    |137.05 |
|2026-10-27 08:05:00|3          |25   |489.24 |
|2026-10-27 08:05:00|4          |8    |129.43 |
|2026-10-27 08:05:00|5          |27   |847.02 |
|2026-10-27 08:05:00|6          |2    |37.08  |
|2026-10-27 08:05:00|7          |4    |89.72  |
|2026-10-27 08:05:00|8          |7    |162.22 |
|2026-10-27 08:05:00|9          |2    |23.64  |
|2026-10-27 08:05:00|10         |7    |108.24 |
|2026-10-27 08:05:00|11         |17   |345.66 |
|2026-10-27 08:05:00|12         |1    |8.18   |
|2026-10-27 08:05:00|13         |9    |180.76 |
|2026-10-27 08:05:00|14         |6    |128.3  |
|2026-10-27 08:05:00|15         |1    |20.22  |
|2026-10-27 08:05:00|16         |7    |138.67 |
|2026-10-27 08:05:00|17         |11   |242.09 |
|2026-10-27 08:05:00|18         |2    |30.99  |
|2026-10-27 08:05:00|19         |9    |172.4  |
|2026-10-27 08:05:00|20         |8    |187.05 |
|2026-10-27 08:05:00|21         |4    |76.83  |
|2026-10-27 08:05:00|22         |5    |133.7  |
|2026-10-27 08:05:00|23         |7    |112.45 |
|2026-10-27 08:05:00|24         |6    |173.02 |
|2026-10-27 08:05:00|25         |8    |203.73 |
|2026-10-27 08:10:00|1          |17   |436.34 |
|2026-10-27 08:10:00|2          |12   |262.29 |
|2026-10-27 08:10:00|3          |21   |532.3  |
|2026-10-27 08:10:00|4          |4    |63.48  |
|2026-10-27 08:10:00|5          |16   |402.58 |
|2026-10-27 08:10:00|6          |4    |132.94 |
|2026-10-27 08:10:00|7          |6    |137.45 |
|2026-10-27 08:10:00|8          |10   |224.55 |
|2026-10-27 08:10:00|9          |3    |68.88  |
|2026-10-27 08:10:00|10         |7    |170.5  |
|2026-10-27 08:10:00|11         |15   |258.12 |
|2026-10-27 08:10:00|12         |4    |80.47  |
|2026-10-27 08:10:00|13         |9    |282.11 |
|2026-10-27 08:10:00|14         |7    |136.48 |
|2026-10-27 08:10:00|15         |6    |136.15 |
|2026-10-27 08:10:00|16         |4    |91.58  |
|2026-10-27 08:10:00|17         |8    |207.18 |
|2026-10-27 08:10:00|18         |6    |164.03 |
|2026-10-27 08:10:00|19         |6    |93.07  |
|2026-10-27 08:10:00|20         |9    |151.73 |
|2026-10-27 08:10:00|21         |5    |182.79 |
|2026-10-27 08:10:00|22         |3    |100.83 |
|2026-10-27 08:10:00|23         |13   |298.0  |
|2026-10-27 08:10:00|24         |2    |15.69  |
|2026-10-27 08:10:00|25         |6    |114.3  |
|2026-10-27 08:15:00|1          |17   |435.59 |
|2026-10-27 08:15:00|2          |7    |98.03  |
|2026-10-27 08:15:00|3          |19   |352.01 |
|2026-10-27 08:15:00|4          |6    |79.94  |
|2026-10-27 08:15:00|5          |22   |618.73 |
|2026-10-27 08:15:00|6          |6    |132.46 |
|2026-10-27 08:15:00|7          |9    |197.79 |
|2026-10-27 08:15:00|8          |12   |249.17 |
|2026-10-27 08:15:00|9          |2    |27.25  |
|2026-10-27 08:15:00|10         |8    |126.58 |
|2026-10-27 08:15:00|11         |15   |325.74 |
|2026-10-27 08:15:00|13         |8    |124.17 |
|2026-10-27 08:15:00|14         |12   |258.01 |
|2026-10-27 08:15:00|15         |1    |14.87  |
|2026-10-27 08:15:00|16         |7    |133.52 |
|2026-10-27 08:15:00|17         |8    |151.48 |
|2026-10-27 08:15:00|18         |4    |59.77  |
+-------------------+-----------+-----+-------+
only showing top 100 rows
```

Which 5-minute windows did the stream write, and which are missing compared with the batch
table printed after the query? For one window that was written, compare its trips with the
batch value: where did the difference come from (look at how `datagen.py` makes the late
events)? Why does `state rows` drop every five batches?

Windows written and missing:
The stream wrote every window from 07:55–08:00 through 08:20–08:25. In append mode, a window is written only after the watermark (max event time − 3 min) passes its end. The final watermark was 08:26:43, so the 08:25–08:30 window was never closed and is missing from the stream output. Its rows are still in state, which is the 25 state rows at the end, one per zone.

One window compared with batch:
Zone 5, 08:05–08:10: the stream has 25 trips (797.30) and the batch has 27 (847.02). In datagen.py, late rides are 1–2 minutes behind, which is within the 3-minute watermark, so they are counted. Very late rides are 8–10 minutes behind, so their window has usually already closed and Spark drops them. The batch query has no watermark, so it counts everything. Across the printed rows, 12 of the 13 very late rides are missing.

Why state rows drop every five batches:
Each file covers about 1 minute, so the watermark moves about 1 minute per batch. Usually two windows are open (about 50 rows = 2 × 25 zones). Every fifth batch the watermark crosses a 5-minute boundary, and the older window is written and removed, which leaves about 25 rows.


## Part 1b: alerts

How many alerts, of which kind, and the first five lines of the output:

```
=== part1b: micro-batch summary ===
batch input rows            watermark state rows
    0          0                    -          -
    1          2                    -          -
    2          0                    -          -
    3          0                    -          -
    4          0                    -          -
    5          0                    -          -
    6          1                    -          -
    7          0                    -          -
    8          1                    -          -
    9          0                    -          -
   10          2                    -          -
   11          0                    -          -
   12          1                    -          -
   13          0                    -          -
   14          1                    -          -
   15          0                    -          -
   16          1                    -          -
   17          0                    -          -
   18          0                    -          -
   19          0                    -          -
   20          1                    -          -
   21          0                    -          -
   22          0                    -          -
   23          0                    -          -
   24          0                    -          -
   25          0                    -          -
   26          1                    -          -
   27          0                    -          -
   28          0                    -          -
   29          0                    -          -

=== part1b: 11 alerts ===
+-------------+-----+
|       reason|count|
+-------------+-----+
|zero_distance|   11|
+-------------+-----+

```

This query has no watermark and no state. Why is that correct here, and what would change if
you also wanted "more than three alerts for the same driver in ten minutes"?
 
Why no watermark or state:
Each ride is checked on its own (fare > 150 or distance = 0), so nothing has to be remembered between batches and nothing has to wait. Each alert is written as soon as the ride arrives, and a late ride is just as valid. A watermark only exists to limit state, and this query has none.

“More than three alerts per driver in ten minutes”:
The query becomes stateful. You would group the alerts by driver_id and window(event_time, "10 minutes"), count them, and keep the groups where count > 3. A sliding window (for example, sliding every 1 minute) catches bursts that cross a window boundary. You would also need a watermark, or the per-driver counts would stay in state forever, and rides later than the watermark would then be dropped. Append mode emits an alert only after the window closes, which is late. Update mode emits as soon as the count passes 3, but the CSV sink only supports append, so you would need foreachBatch or a Kafka sink.



---

## Part 2: the fare model

The three lines printed by `evaluate()` (also in `shared-folder/output/part2/metrics`), the
"tuned" line with the best parameters, and the "best model" line:

```
seed 801505751: 47983 training rows, 12017 test rows
baseline   RMSE   3.71   R2  0.850
full       RMSE   1.71   R2  0.968
tuned      best params: numTrees=50, maxDepth=10; 4 combinations x 3 folds + 1 refit = 13 fits
tuned      RMSE   2.43   R2  0.936
best model: full; part 2 took 170 s
```

Your tuned model: which estimator, which parameters in the grid and why those, how many fits
the CrossValidator ran, and how long part 2 took. Did the tuning beat the full linear
pipeline? If not, what does that tell you about the data?

Tuned model:
I used a RandomForestRegressor on zone_idx, payment_type_idx, hour_idx, distance_km and duration_min. Trees split on the category indexes directly, so no one-hot encoding is needed. I set maxBins=32 so the 25 zones (plus the “keep” bucket) can all be separated. The grid was numTrees ∈ {20, 50}, since more trees give more stable predictions, and maxDepth ∈ {6, 10}, since deeper trees fit finer detail. CrossValidator ran 4 combinations × 3 folds + 1 refit = 13 fits, and part 2 took 170 s. The best combination was numTrees=50 and maxDepth=10.

Did tuning beat the full pipeline?
No. The forest’s RMSE was 2.43, against 1.71 for the full linear pipeline. In datagen.py the fare is (2.5 + 1.6·km + 0.35·min) × surge + zone premium + cash extra + noise (σ = 1.5), which is almost a straight line. The linear model fits it closely, and 1.71 is near the noise level of 1.5. A forest can only approximate a straight line with steps, so it keeps extra error. Flexible models help when the data is strongly non-linear, and this data mostly isn’t.

Why is `hour` turned into a string before `StringIndexer` (see `features_of`) instead of
being used as a number?

Why hour is a string:
As a number, hour would get one slope, so every extra hour would add the same amount to the fare. In datagen.py only hours 7, 8, 17 and 18 have a surge, so the hour effect jumps up and down with no order. As a string, StringIndexer and OneHotEncoder give each hour its own coefficient.


---

## Part 3: scoring the stream

The threshold used, the number of rides flagged, the ten rides with the largest deviation
and the per-window counts:

```
=== part3: micro-batch summary ===
batch input rows            watermark state rows
    0         40                    -          -
    1         45                    -          -
    2         42                    -          -
    3         41                    -          -
    4         40                    -          -
    5         35                    -          -
    6         45                    -          -
    7         41                    -          -
    8         43                    -          -
    9         37                    -          -
   10         44                    -          -
   11         40                    -          -
   12         38                    -          -
   13         37                    -          -
   14         39                    -          -
   15         39                    -          -
   16         35                    -          -
   17         36                    -          -
   18         44                    -          -
   19         45                    -          -
   20         45                    -          -
   21         35                    -          -
   22         40                    -          -
   23         38                    -          -
   24         39                    -          -
   25         43                    -          -
   26         36                    -          -
   27         38                    -          -
   28         43                    -          -
   29         39                    -          -

=== part3: 65 rides flagged ===
+-------+-------------------+-----------+-----------+------------+------------+-----+----------+---------+
|ride_id|event_time         |pickup_zone|distance_km|duration_min|payment_type|fare |prediction|deviation|
+-------+-------------------+-----------+-----------+------------+------------+-----+----------+---------+
|R00663 |2026-10-27 08:15:57|7          |7.15       |24.1        |card        |80.27|26.81     |53.46    |
|R01033 |2026-10-27 08:25:22|2          |6.56       |29.5        |card        |80.39|27.84     |52.55    |
|R00172 |2026-10-27 08:04:32|23         |5.21       |21.8        |card        |73.35|22.69     |50.66    |
|R00874 |2026-10-27 08:21:27|25         |5.9        |15.2        |card        |63.69|21.47     |42.22    |
|R00481 |2026-10-27 08:11:12|13         |4.87       |20.8        |app         |57.94|21.71     |36.23    |
|R00185 |2026-10-27 08:03:45|1          |4.33       |15.3        |card        |57.55|21.36     |36.19    |
|R00797 |2026-10-27 08:19:22|5          |2.81       |11.8        |card        |56.13|24.04     |32.09    |
|R00300 |2026-10-27 08:07:07|11         |3.49       |13.8        |card        |44.98|16.86     |28.12    |
|R00434 |2026-10-27 08:09:44|22         |5.45       |21.2        |app         |50.78|22.83     |27.95    |
|R00837 |2026-10-27 08:18:54|1          |3.6        |13.4        |card        |46.83|19.45     |27.38    |
+-------+-------------------+-----------+-----------+------------+------------+-----+----------+---------+
only showing top 10 rows
=== part3: flagged rides per 5-minute window (batch over the output) ===
+-------------------+-----+
|       window_start|count|
+-------------------+-----+
|2026-10-27 07:55:00|    1|
|2026-10-27 08:00:00|   10|
|2026-10-27 08:05:00|   17|
|2026-10-27 08:10:00|   14|
|2026-10-27 08:15:00|    9|
|2026-10-27 08:20:00|   10|
|2026-10-27 08:25:00|    4|
+-------------------+-----+


```

Run `python3 datagen.py 801505751 shared-folder /tmp/truth.csv` (the third argument writes the
list of planted anomalies, which the generator does not reveal otherwise) and compare it with
your flagged rides: how many of the planted anomalies did you catch, and how many flagged
rides were not planted? Look at the false positives: what do they have in common, and what
would you change in part 2 to reduce them?

``seed 801505751: 60000 historical trips, 30 stream files with 1202 rides (43 fares that do not match the trip, 57 late by 1-2 minutes, 13 late by 8-10 minutes, 11 with zero distance)``

With a threshold of 4 × 1.71 = 6.84, part 3 flagged 65 rides. It caught 42 of the 43 planted anomalies (recall 98%), and 23 flagged rides were not planted (precision 65%). The top 10 deviations (27–53) are all planted: their fares are 2–3 times the prediction.

What the false positives have in common:
In all 23, the real fare is higher than the prediction.

18 are long trips (8.7–29.4 km). The stream runs from 08:00 to 08:30, a surge hour, and in datagen.py the surge multiplies the per-km and per-minute rates by 1.25. The linear model only adds a fixed extra for hour 8, so it underpredicts more the longer the trip is.
5 are zero-distance rides. datagen.py computes their fare from the real distance and then sets distance_km to 0. The model predicts from 0 km, so it comes out too low.

What to change in part 2:

Add distance × is_rush and duration × is_rush features (is_rush = hour 7, 8, 17 or 18), or train on log(fare).
Don’t score zero-distance rides with the fare model; leave them to the part 1b rule.

The one missed anomaly is probably a “low” fare (multiplied by 0.2–0.4) on a short trip, so its deviation stayed below 6.84.

Why is there no `fit()` in part 3, and what would happen if you tried one on the stream?
Which stages of the loaded model are fitted models, and what did each one learn from the
training data?

Why there is no fit() in part 3:
The model was trained once, offline, on clean history. Part 3 only applies it with transform(). fit() needs a complete, finite dataset, and a stream never ends, so Spark raises an error:
AnalysisException: Queries with streaming sources must be executed with writeStream.start().
Even if it worked, the model would learn from the anomalies it is supposed to catch.

Fitted stages of the loaded model:

StringIndexerModel: for zone, payment_type and hour, the mapping from each label to an index, ordered by frequency. Unseen labels go to the extra “keep” index.
OneHotEncoderModel: the number of categories in each column, which sets the length of each one-hot vector.
VectorAssembler: not fitted. It only joins the columns into one feature vector.
LinearRegressionModel: the intercept, plus one coefficient for each zone, payment type and hour, and one each for km and minutes.


---

## Design: this pipeline in production

One paragraph. The platform runs this for real: thousands of rides a minute from Kafka, a
model retrained every night on the trips of the last month, anomalies sent to a fraud team.
Where do the training job, the scoring job, the checkpoint and the model live, how does a
new model reach the running scoring job without losing events, and what would you monitor
to know that the model has gone stale?

The training job is a nightly batch Spark job, run by a scheduler such as Airflow. It reads the last month of trips from the data lake (S3/HDFS), fits the pipeline, and saves the new PipelineModel to versioned storage (e.g. models/fare/v=2026-11-05/), but only if it is no worse than the current model on a hold-out set. The scoring job is a long-running Structured Streaming job that reads rides from Kafka and sends flagged rides to a Kafka topic for the fraud team. Its checkpoint lives on durable storage (S3/HDFS, not local disk), so it keeps the Kafka offsets across restarts. To switch to a new model, the scoring job restarts from the same checkpoint and continues from the last committed offset, so no event is lost. Alternatively, a foreachBatch reloads the model between micro-batches. To know when the model has gone stale, I would monitor the rolling RMSE against the training RMSE, the flag rate per hour, drift in distance, duration and zone mix, the fraud team’s confirmation rate, and Kafka consumer lag.


---

## Problems and fixes

What went wrong and how you fixed it, with the actual error messages. If nothing went wrong,
say so.
 no problems encountered
