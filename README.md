# Shop-Counter
Python Code using Google Maps API to count types of businesses in a location

import googlemaps
import time

# Set Google Maps API Key
API_KEY = 'YOUR_API_HERE'

# Initialize the Google Maps client
gmaps = googlemaps.Client(key=API_KEY)

# Coordinates for Minneapolis-St. Paul metro area
location = (44.9778, -93.2650)  # Approximate central location for Minneapolis

# Define the search radius in meters (you can adjust this as needed)
radius = 50000  # 50 km

# Function to count barbershops
def count_barbershops(location, radius):
    barbershops_count = 0
    next_page_token = None

    while True:
        if next_page_token:
            places_result = gmaps.places_nearby(
                location=location,
                radius=radius,
                keyword='barbershop',
                page_token=next_page_token
            )
        else:
            places_result = gmaps.places_nearby(
                location=location,
                radius=radius,
                keyword='barbershop'
            )

        barbershops_count += len(places_result['results'])

        next_page_token = places_result.get('next_page_token')
        if not next_page_token:
            break

        time.sleep(2)  # Pause to respect API rate limits

    return barbershops_count

# Count the barbershops
barbershops_count = count_barbershops(location, radius)
print(f'Total number of barbershops in the Minneapolis-St. Paul metro area: {barbershops_count}')

**Total number of barbershops in the Minneapolis-St. Paul metro area: 60**

