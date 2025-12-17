#!/usr/bin/env python
"""
File Extension Report
- Scans the repository
- Counts files by extension
- Saves results into extensions.json
"""

import os, json, datetime
from collections import Counter

def main():
    counter = Counter()

    for root, _, files in os.walk("."):
        if ".git" in root:
            continue
        for name in files:
            ext = os.path.splitext(name)[1].lower() or "no_extension"
            counter[ext] += 1

    report = {
        "generated_at": datetime.datetime.now().isoformat(timespec="seconds"),
        "total_files": sum(counter.values()),
        "by_extension": dict(counter)
    }

    with open("extensions.json", "w", encoding="utf-8") as f:
        json.dump(report, f, indent=2)

    print("Extension report saved to extensions.json")
    for ext, count in counter.most_common():
        print(f"{ext}: {count}")

if __name__ == "__main__":
    main()
