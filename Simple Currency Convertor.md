import requests

base_url = "https://api.exchangerate-api.com/v4/latest"  
# Base URL for the exchange rate API

# Gathering the Parameters
date = input("Please enter the date (in the format 'yyyy-mm-dd' or 'latest'): ")
base = input("Convert from (currency): ").upper()  # Convert input to uppercase for consistency
curr = input("Convert to (currency): ").upper()  # Convert input to uppercase for consistency
quan = float(input(f"How much {base} do you want to convert? "))

# Constructing a URL and sending the GET request
url = f"{base_url}/{base}"
params = { "symbols": curr}
response = requests.get(url, params=params)

# Handling the Response
if not response.ok:
    print(f"\nError {response.status_code}:")
    print(response.text)  # Print the error response text for debugging
else:
    try:
        data = response.json()
        if 'error' in data:
            print(f"\nError from API: {data['error']}")
        else:
            rate = data['rates'][curr]
            result = quan * rate
            print(f"\n{quan} {base} is equal to {result} {curr}, based upon exchange rates on {data['date']}")
    except requests.exceptions.RequestException as e:
        print(f"\nRequest Exception: {e}")
    except (KeyError, ValueError) as e:
        print(f"\nError parsing response: {e}")
