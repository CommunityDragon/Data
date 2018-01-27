# Data
A place for keeping and discussing additional data we plan to provide through CDragon.

## patches.json
This file contains the start and end timestamps for each patch, back to preseason 3.

The patches are contained in a list accessible through the `patches` key. Each patch has `start` and `end` times (as ints), a `name` (str), and a `season` ID (int). In additional, the regional offsets are provided (in seconds) for each platform and are accessible via the key `shifts`.

All timestamps are in UTC time.

Below is an example of loading the start time for the most recent patch in Python:

```python
import arrow, json

with open('patches.json') as f:
    data = json.load(f)

patch = data['patches'][-1]  # most recent
utc_timestamp = patch['start']
north_america_timestamp = utc_timestamp + data['shifts']['NA1']
dt = arrow.get(north_america_timestamp)
print(dt.to('US/Pacific'))  # 2018-01-24T03:00:00-08:00  (for patch 8.2)
# which corresponds to 3am on January 24th, 2018 in Pacific Time.
