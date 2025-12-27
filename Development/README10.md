This Launcher script is the "brain" of Oahu Maps. It acts as a bridge between your saved knowledge of Python libraries and your three distinct projects.

By using a simple conditional setup, you can switch between Shop Oahu, Phoenix Valley, or localSq with a single variable change.

🚀 The 'Oahu Maps' Project Launcher 
This script pulls in the Fetcher, Cache, and Engine modules we've built this morning.

```
def run_oahu_maps_project(project_name):
    # 1. Initialize our core modules
    fetcher = OahuDataFetcher()
    cache = OahuCache()
    
    print(f"--- Launching {project_name} Mapping Engine ---")

    if project_name == "Shop Oahu":
        # Honolulu center
        engine = OahuMapsEngine(location=[21.3069, -157.8583], zoom=12)
        # Fetch and Cache
        data = cache.get_data("hnl_retail.json", fetcher.fetch_honolulu_shops)
        output_file = "shop_oahu_discovery.html"

    elif project_name == "Phoenix Valley":
        # Phoenix center
        engine = OahuMapsEngine(location=[33.4484, -112.0740], zoom=11)
        # Fetch and Cache
        data = cache.get_data("phx_landmarks.json", fetcher.fetch_phoenix_landmarks)
        output_file = "phoenix_valley_overview.html"

    elif project_name == "localSq":
        # Flexible - let's default to Oahu for now
        engine = OahuMapsEngine(location=[21.3069, -157.8583], zoom=13)
        # localSq might use a local JSON file of user check-ins
        data = cache.get_data("localsq_users.json", lambda: [{"name": "User 1", "lat": 21.3, "lon": -157.8}])
        output_file = "localsq_activity.html"

    # 2. Process data through the Engine
    engine.add_data_points(data)
    engine.add_search_bar()
    
    # 3. Generate the final interactive product
    engine.finalize(filename=output_file)

# --- Execution ---
if __name__ == "__main__":
    # Just change this string to switch projects!
    run_oahu_maps_project("Shop Oahu")
```


📂 Your Project Directory Structure 
To keep this organized as you build, I recommend a folder structure like this:
 * oahu_maps/
   * main.py (The Launcher above)
   * engine.py (The Folium logic)
   * data_utils.py (Fetcher & Cache)
   * map_cache/ (Where your JSON/XML files live)
   * outputs/ (The generated .html maps)

🌟 Why this matters for your 2025 goals 
Since it's late December, you're in a great position to start 2026 with a unified geospatial toolkit. Instead of having three messy mapping implementations, you have one high-performance engine that:
 * Handles XML/KML (crucial for government data).
 * Caches API responses (saves time and money).
 * Is searchable and interactive (better user experience).

Would you like me to generate a "Requirements.txt" file for you so you have all the specific library versions ready to install for this project?
