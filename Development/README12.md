A "Data Cleaner" is essential because real-world data from city portals is often messy. You might find rows with missing coordinates, "None" values in shop names, or coordinates swapped (Longitude where Latitude should be).

If you feed "dirty" data into Oahu Maps, the map might crash or markers will end up in the middle of the ocean!

🧹 The 'Oahu Maps' Data Cleaner 
We can add this as a static method in our OahuDataFetcher class. It will act as a "filter" that only lets high-quality data through to Shop Oahu or Phoenix Valley.

```
class DataCleaner:
    @staticmethod
    def clean_records(raw_data):
        """
        Validates and formats data for the mapping engine.
        Removes duplicates and entries with invalid coordinates.
        """
        clean_list = []
        seen_names = set()

        for entry in raw_data:
            try:
                # 1. Extract and convert coordinates to float
                # Handles cases where data might be strings or None
                lat = float(entry.get('latitude') or entry.get('lat'))
                lon = float(entry.get('longitude') or entry.get('lon'))
                name = str(entry.get('name') or entry.get('Name') or "Unnamed Location")

                # 2. Boundary Check (Safety Check)
                # Ensures points are actually near Hawaii or Arizona
                is_hi = (18.0 < lat < 23.0) and (-160.0 < lon < -154.0)
                is_az = (31.0 < lat < 37.0) and (-115.0 < lon < -109.0)

                if not (is_hi or is_az):
                    continue # Skip if the point is in the middle of the ocean/wrong state

                # 3. Deduplication
                if name in seen_names:
                    continue
                
                clean_list.append({
                    "name": name.strip().title(),
                    "lat": lat,
                    "lon": lon,
                    "popup": f"<b>{name}</b><br>Verified Location"
                })
                seen_names.add(name)

            except (TypeError, ValueError):
                # Skip rows with non-numeric coordinates or missing data
                continue

        print(f"🧹 Cleaned {len(raw_data)} raw records down to {len(clean_list)} valid points.")
        return clean_list
```


🏛️ Why this is a lifesaver for your projects: 
 * Shop Oahu: Government lists of businesses often include closed shops or administrative entries without addresses. This cleaner skips those so your map stays accurate.
 * Phoenix Valley: If you're pulling XML/KML data for various "Valley" cities (Tempe, Scottsdale, Mesa), this script ensures they all get standardized into one format.
 * localSq: When users input their own data, they might make typos. This cleaner ensures only valid GPS points hit your database.

🌟 Pro Tip: The "Boundary Check" 
Notice the is_hi and is_az checks. Since you are building a tool for two specific regions, this prevents "rogue" data points from appearing. If a dataset accidentally includes a shop in New York, the cleaner will catch it before it messes up your Oahu-centric view.

🚀 What's next for Oahu Maps? 
We have a very robust pipeline now: Fetch → Cache → Clean → Engine → Interactive HTML.

Would you like me to create a "Dashboard Template"? This would be a simple Python script using Streamlit (another great library) that lets you use a dropdown menu to select your project and view the map instantly in a sidebar.
