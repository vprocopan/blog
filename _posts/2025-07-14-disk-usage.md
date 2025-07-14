To find out which directory occupies the most space in your root (`/`) filesystem, use:

```bash
sudo du -h --max-depth=1 / | sort -hr | head -n 10
```

### Explanation:

* `sudo`: Ensures you have permission to read all directories.
* `du -h --max-depth=1 /`: Shows disk usage for each top-level directory under `/` in human-readable format.
* `sort -hr`: Sorts the output by size in human-readable format (e.g., `10G` > `500M`).
* `head -n 10`: Shows the top 10 largest directories.

This will help you quickly identify which top-level folders (like `/var`, `/usr`, `/home`) are taking up the most space.
