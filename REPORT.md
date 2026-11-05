# Assignment #4: Report

**Name:**
**Student ID:**
**Email:**

---

## Seed and commands

Seed used for `datagen.py`:

The commands you ran, in order:

```bash

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
```

Which 5-minute windows did the stream write, and which are missing compared with the batch
table printed after the query? For one window that was written, compare its trips with the
batch value: where did the difference come from (look at how `datagen.py` makes the late
events)? Why does `state rows` drop every five batches?



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



---

## Part 2: the fare model

The three lines printed by `evaluate()` (also in `shared-folder/output/part2/metrics`), the
"tuned" line with the best parameters, and the "best model" line:

```
tuned      best params: numTrees=50, maxDepth=10; 4 combinations x 3 folds + 1 refit = 13 fits
tuned      RMSE   2.43   R2  0.936
best model: full; part 2 took 170 s
```

Your tuned model: which estimator, which parameters in the grid and why those, how many fits
the CrossValidator ran, and how long part 2 took. Did the tuning beat the full linear
pipeline? If not, what does that tell you about the data?



Why is `hour` turned into a string before `StringIndexer` (see `features_of`) instead of
being used as a number?



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

Run `python3 datagen.py <seed> shared-folder /tmp/truth.csv` (the third argument writes the
list of planted anomalies, which the generator does not reveal otherwise) and compare it with
your flagged rides: how many of the planted anomalies did you catch, and how many flagged
rides were not planted? Look at the false positives: what do they have in common, and what
would you change in part 2 to reduce them?



Why is there no `fit()` in part 3, and what would happen if you tried one on the stream?
Which stages of the loaded model are fitted models, and what did each one learn from the
training data?



---

## Design: this pipeline in production

One paragraph. The platform runs this for real: thousands of rides a minute from Kafka, a
model retrained every night on the trips of the last month, anomalies sent to a fraud team.
Where do the training job, the scoring job, the checkpoint and the model live, how does a
new model reach the running scoring job without losing events, and what would you monitor
to know that the model has gone stale?



---

## Problems and fixes

What went wrong and how you fixed it, with the actual error messages. If nothing went wrong,
say so.

