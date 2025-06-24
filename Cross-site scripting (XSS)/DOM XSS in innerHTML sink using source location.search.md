## Solution steps

- Enter the following into the into the search box: `<img src=1 onerror=alert(1)>`
- Click "Search".
- This is what happens:
  1. Browser tries to load the image from src=1 (invalid).
  2. Since it fails to load, the onerror event is triggered.
  3. Your onerror=alert(1) executes.
