# Rumble Video Lister

This is a small research tool for listing metadata of all accessible Rumble videos within a certain video ID range, including unlisted and restricted (embed-only) videos. The tool performs a multi-threaded scan of the sequential video IDs with minimal web traffic using mostly the embed API and HTTP headers. Under good network conditions, around 10-15 IDs per second can be queried from one IP address.

![rumble-lister](https://github.com/user-attachments/assets/aa7633c3-fac6-4b21-ad2b-20db0144de5b)

## Features

The tool displays a metadata list of discovered videos, which can be optionally logged to a text file. Restricted/anonymized (embed-only) videos do not have descriptive metadata (title or channel name), therefore the video thumbnails can be optionally downloaded into a folder (the file name being the embed video ID) to infer the content. The collected metadata includes:
- Timestamp (either the time of the upload or the scheduled publication time)
- Video duration
- Channel name (for restricted/anonymized videos always "hostid1")
- Title (for restricted/anonymized videos this only includes a sequential numeric ID)
- Canonical video URL (for restricted/anonymized videos the embed URL, since the canonical URL appears as private)

In addition, the tool allows searching for (fully) privated video URLs (no metadata are available) but at a significantly slower rate, since this is only possible by querying the canonical (rather than the embed) URL, which has much tighter rate limits.

## Usage

Labview 2023 or later is required to view and run the code. You can use the free [community edition](https://www.ni.com/en/support/downloads/software-products/download.labview-community.html). It should run on all supported Labview platforms, but it was only tested on Windows. Save all *.vi files into the same folder (raw data mode or download repo as zip) and open the "Rumble lister.vi" front panel. Enter the 6-character *canonical* (not embed) start and end video ID of the range you want to scan (if the start ID is later than the end ID, the scan is done backwards). A canonical video ID can be found in every video URL on the Rumble website (`rumble.com/v######-.html`) or by clicking the Rumble link in the embedded player. Optionally, choose a path to a log file to save the output in, and a folder for restricted/anonymized video thumbnails (paths cannot be changed during the scan). Then run the vi (Ctrl+R) to start the scan. You can enable/disable saving thumbnails as well as detecting private video IDs during the scan. If you get a rate limit warning or errors, try reducing the number of threads or adding a delay time.

Note that the metadata list is not strictly sorted by video ID, since it is generated live from the multi-threaded scan. Consecutive video IDs can be separated by up to the number of threads.
