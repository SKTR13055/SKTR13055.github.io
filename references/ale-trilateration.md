---
layout: page
title: Ale's Trilateration Solver
permalink: /references/ale-trilateration/
---

## 🌍 OSINT Trilateration Tool

This script helps locate a target by calculating the intersection of distances from three specific amenities (bars, restaurants, etc.) in a city. It pulls data from the **Overpass API**.

**Credit:** Script provided by *ale* for the "Best Food" challenge.

```python
import math
import time
import requests

OVERPASS = "[https://overpass-api.de/api/interpreter](https://overpass-api.de/api/interpreter)"
EARTH_R = 6371000.0  # meters

def overpass_query(query):
    r = requests.post(OVERPASS, data={"data": query}, timeout=60)
    r.raise_for_status()
    return r.json()

def build_query(city, amenity):
    return f"""
[out:json][timeout:25];
area[name="{city}"]->.a;
node["amenity"="{amenity}"](area.a);
out;
"""

def fetch_coords(city, amenity):
    q = build_query(city, amenity)
    data = overpass_query(q)
    coords = [(e["lat"], e["lon"]) for e in data.get("elements", []) if "lat" in e]
    return coords

def mean_center(points):
    if not points:
        return None
    lat = sum(p[0] for p in points)/len(points)
    lon = sum(p[1] for p in points)/len(points)
    return (lat, lon)

def latlon_to_xy(lat, lon, lat0, lon0):
    lat, lon = map(math.radians, (lat, lon))
    lat0, lon0 = map(math.radians, (lat0, lon0))
    x = EARTH_R * (lon - lon0) * math.cos((lat + lat0)/2)
    y = EARTH_R * (lat - lat0)
    return x, y

def xy_to_latlon(x, y, lat0, lon0):
    lat0_r, lon0_r = map(math.radians, (lat0, lon0))
    lat = y/EARTH_R + lat0_r
    lon = x/(EARTH_R*math.cos((lat+lat0_r)/2)) + lon0_r
    return math.degrees(lat), math.degrees(lon)

def trilaterate(a, b, c):
    x1,y1,r1 = a
    x2,y2,r2 = b
    x3,y3,r3 = c

    dx = x2-x1
    dy = y2-y1
    d = math.hypot(dx,dy)
    if d == 0:
        raise ValueError("Points 1 and 2 identical")

    ex = (dx/d, dy/d)
    i = ex[0]*(x3-x1) + ex[1]*(y3-y1)

    px = x3 - x1 - i*ex[0]
    py = y3 - y1 - i*ex[1]
    j = math.hypot(px, py)
    if j == 0:
        raise ValueError("Degenerate configuration")

    ey = (px/j, py/j)

    x = (r1*r1 - r2*r2 + d*d) / (2*d)
    y = (r1*r1 - r3*r3 + i*i + j*j - 2*i*x) / (2*j)

    fx = x1 + x*ex[0] + y*ey[0]
    fy = y1 + x*ex[1] + y*ey[1]
    return fx, fy

def main():
    print("---------------------------------------")
    print(" OSINT Trilateration Solver (General)")
    print("---------------------------------------\n")

    # ---- USER INPUT ----
    city = input("Enter city (e.g., Graz): ").strip()

    a1 = input("Amenity 1 (e.g., bar): ").strip()
    a2 = input("Amenity 2: ").strip()
    a3 = input("Amenity 3: ").strip()

    d1 = float(input("Distance to amenity 1: "))
    d2 = float(input("Distance to amenity 2: "))
    d3 = float(input("Distance to amenity 3: "))

    unit = input("Units? (km / m): ").strip().lower()
    if unit == "km":
        d1*=1000; d2*=1000; d3*=1000
    elif unit != "m":
        print("Invalid unit")
        return

    print("\nFetching amenity coordinates from OpenStreetMap...\n")

    coords1 = fetch_coords(city, a1); time.sleep(1)
    coords2 = fetch_coords(city, a2); time.sleep(1)
    coords3 = fetch_coords(city, a3); time.sleep(1)

    if not coords1 or not coords2 or not coords3:
        print("ERROR: One of the amenities returned zero results.")
        return

    m1 = mean_center(coords1)
    m2 = mean_center(coords2)
    m3 = mean_center(coords3)

    lat0 = (m1[0] + m2[0] + m3[0]) / 3
    lon0 = (m1[1] + m2[1] + m3[1]) / 3

    x1,y1 = latlon_to_xy(*m1, lat0, lon0)
    x2,y2 = latlon_to_xy(*m2, lat0, lon0)
    x3,y3 = latlon_to_xy(*m3, lat0, lon0)

    fx, fy = trilaterate((x1,y1,d1), (x2,y2,d2), (x3,y3,d3))

    latf, lonf = xy_to_latlon(fx, fy, lat0, lon0)

    print("\n---------------------------------------")
    print("         FINAL COORDINATES")
    print("---------------------------------------")
    print(f"Latitude:  {latf:.8f}")
    print(f"Longitude: {lonf:.8f}")
    print("\nPaste these into Google Maps/OpenStreetMap.")
    print("Look for a restaurant there and check **reviews** for the flag!\n")

if __name__ == "__main__":
    main()
