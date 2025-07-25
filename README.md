# crypto-price-tracker
🧰 Requirements

Install Streamlit and Requests:

pip install streamlit requests


🧾 Script: crypto_table_tracker.py

import streamlit as st
import requests

st.title("💰 Live Crypto Prices")

# Select coins
coin_names = {
    "Bitcoin": "bitcoin",
    "Ethereum": "ethereum",
    "Dogecoin": "dogecoin",
    "Cardano": "cardano",
    "Solana": "solana"
}

selected = st.multiselect("Select Cryptocurrencies", list(coin_names.keys()), default=["Bitcoin", "Ethereum"])

# Refresh interval
refresh = st.slider("Refresh every N seconds", 10, 300, 60)

# Get CoinGecko IDs
coin_ids = [coin_names[c] for c in selected]

# Function to fetch prices
def fetch_prices(ids):
    url = f"https://api.coingecko.com/api/v3/simple/price?ids={','.join(ids)}&vs_currencies=usd"
    r = requests.get(url)
    return r.json()

# Show table
if st.button("▶ Start Tracking"):
    placeholder = st.empty()
    st.info("Tracking... Click stop to exit")

    try:
        while True:
            data = fetch_prices(coin_ids)
            table = [[name, f"${data[coin_names[name]]['usd']:.2f}"] for name in selected]
            placeholder.table(table)
            st.sleep(refresh)
    except:
        st.warning("Stopped tracking.")
